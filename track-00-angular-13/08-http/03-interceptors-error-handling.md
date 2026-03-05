# Advanced Features

# 1️⃣ HTTP Interceptors

## Definition

An HTTP Interceptor in Angular is a service that intercepts outgoing
HTTP requests and incoming responses.

It allows you to:

-   Modify requests
-   Add headers (e.g., Auth tokens)
-   Handle errors globally
-   Log traffic
-   Transform responses

Interceptors act as middleware between your app and backend.

------------------------------------------------------------------------

## Analogy

Think of airport security:

-   Every passenger (HTTP request) passes through security.
-   Security checks and modifies if necessary.
-   Only then the passenger proceeds.

Interceptors process every request before it reaches the server.

------------------------------------------------------------------------

## Production-Level Example

### Step 1: Create Auth Interceptor

In Angular, requests are immutable. To change a request, you must .clone() it. Here, we retrieve a fake token and inject it into the headers.

``` ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  authToken!: string | null;
  constructor() {}

  intercept(
    request: HttpRequest<unknown>,
    next: HttpHandler,
  ): Observable<HttpEvent<unknown>> {
    this.authToken = localStorage.getItem('authToken');

    const authToken = request.clone({
      setHeaders: {
        Authorization: `Bearer : ${this.authToken}`,
      },
    });

    console.log('Interceptor: Token attached to request!!');

    return next.handle(authToken);
  }
}
```

### Component & Tempalte Implementation 

```ts
@Component({
  selector: 'app-user',
  template: `
    <div *ngIf="errorMessage" class="alert alert-danger">
      {{ errorMessage }}
    </div>

    <ul class="list-group" *ngIf="users.length > 0">
      <li class="list-group-item" *ngFor="let user of users">
        {{ user | json }}
      </li>
    </ul>
  `,
})
export class UserComponent implements OnInit {
  users: User[] = [];
  errorMessage: string = '';

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    localStorage.setItem('authToken', 'WELCOME_12345');

    this.fetch();
  }

  fetch() {
    this.userService.getUsers().subscribe({
      next: (response) => {
        if (response) {
          this.users = response.splice(0,3);
        } else {
          this.errorMessage = response;
        }
      },
      error: (err) => {
        this.errorMessage = 'Network Error: ' + err.message;
      },
    });
  }
}
````

### The Simple Service Flow

Because the interceptor handles the headers, your service stays completely clean!

````ts
@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private url: string,
  ) {}

  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.url);
  }
}

````

------------------------------------------------------------------------

### Final Step Provide Interceptor

``` ts
import { HTTP_INTERCEPTORS } from '@angular/common/http';

@NgModule({
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ]
})
export class AppModule {}
```

### Output

<img width="1297" height="685" alt="image" src="https://github.com/user-attachments/assets/c26faf26-33de-46e1-9840-8cc841c45b93" />


### Why is this better?

- Maintenance: If your token format changes, you only fix it in one file (the interceptor) instead of 50 different services.

- Security: You ensure that every request is authenticated by default without relying on developers to remember to add headers manually.

- Clean Code: Your business logic (getting posts) is separated from your security logic (adding tokens).

------------------------------------------------------------------------

## Production Use Cases

-   Attach JWT token automatically
-   Global loading spinner
-   Logging API traffic
-   Centralized response transformation

------------------------------------------------------------------------

# 2️⃣ Error Handling

## Definition

Angular provides HttpErrorResponse to handle HTTP errors.

Errors can be handled:

-   Per request
-   Globally using Interceptors
-   Using RxJS catchError operator

------------------------------------------------------------------------

## Analogy

Think of a customer service desk.

-   If something goes wrong, the desk handles complaints.
-   Instead of each department handling separately, centralize error
    handling.

------------------------------------------------------------------------

## 1. Create a Communication Service (error-handler.service.ts)

This service acts as a bridge. The interceptor sends the error to the service, and the template listens for the error from the service.

``` ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class GlobalService {
  private errorSubject = new Subject<string>();

  error$ = this.errorSubject.asObservable();

  constructor() {}

  showError(message: string) {
    this.errorSubject.next(message);
  }

  clearError() {
    this.errorSubject.next('');
  }
}

```

------------------------------------------------------------------------

## 2. The Global Error Logic (error.interceptor.ts)

We use the catchError operator from RxJS. This "listens" for any failed requests coming back from the server.

``` ts
@Injectable()
export class GlobalErrorInterceptor implements HttpInterceptor {
  errorMessage = '';

  constructor(private route: Router) {}

  intercept(
    request: HttpRequest<unknown>,
    next: HttpHandler,
  ): Observable<HttpEvent<unknown>> {
    return next.handle(request).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          this.errorMessage = 'Session Experired, redirect to Login';
          this.route.navigate(['/login']);
        } else if (error.status === 404) {
          this.errorMessage = 'Page Does not exist!, 404 NOT FOUND';
        } else if (error.status === 500) {
          this.errorMessage = 'Server is down! Please try Again Later...';
        } else {
          this.errorMessage = `Error Code ${error.status} \n Error Message ${error.message}`;
        }

        alert(this.errorMessage);

        return throwError(() => new Error(this.errorMessage));
      }),
    );
  }
}

```

------------------------------------------------------------------------

## 3. Register the Interceptor (app.module.ts)

``` ts
  providers: [
    {provide:API_URL,useValue:'https://jsonplaceholder.typicode.com/posts4651456'},
    {provide:HTTP_INTERCEPTORS,useClass:AuthInterceptor,multi:true},
    {provide:HTTP_INTERCEPTORS,useClass:GlobalErrorInterceptor,multi:true}
  ],
```


## 4. Create a Global Error Component
This component will sit at the top of your app (usually in app.component.html) and wait for errors to appear.

````ts
@Component({
  selector: 'app-global-component',
  templateUrl: './global-component.component.html',
  styleUrls: ['./global-component.component.css'],
})
export class GlobalComponentComponent implements OnInit {
  message: string = '';

  constructor(private globalService: GlobalService) {}

  ngOnInit(): void {
    this.globalService.showError(this.message);

    if (this.message) {
      setTimeout(() => this.globalService.clearError(), 50000);
    }
  }

  close() {
    this.globalService.clearError();
  }
}
````

### Template View

````html
<div *ngIf="message" class="alert alert-danger alert-dismissible fade show fixed-top m-3 shadow" role="alert">
  <strong>Error:</strong> {{ message }}
  <button type="button" class="btn-close" (click)="close()" aria-label="Close"></button>
</div>
````

### Output

#### 1. How to trigger a 404 Not Found

Simply change your API URL to a path that doesn't exist.

<img width="1306" height="610" alt="image" src="https://github.com/user-attachments/assets/b12daf80-ac53-4f5a-94b2-0c4689757a6b" />

#### 2. How to trigger a Status 0 (Network Error)

If you want to see what happens when the internet is down or the URL is completely wrong:

<img width="962" height="500" alt="image" src="https://github.com/user-attachments/assets/a781faab-607b-48f5-9a6e-6cf7f75107cd" />

------------------------------------------------------------------------

# Production Best Practices

-   Use Interceptors for global logic.
-   Use catchError inside services.
-   Never swallow errors silently.
-   Show user-friendly error messages.
-   Log critical errors to monitoring tools.
-   Separate technical error details from UI messages.

------------------------------------------------------------------------

## Key Takeaways

-   Interceptors act as middleware.
-   Useful for auth tokens and global logic.
-   HttpErrorResponse provides structured error details.
-   catchError enables reactive error handling.
-   Essential for enterprise-grade Angular applications.
