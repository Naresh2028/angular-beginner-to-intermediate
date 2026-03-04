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

# Using HttpErrorResponse

``` ts
import { HttpErrorResponse } from '@angular/common/http';

this.http.get('api/users').subscribe({
  next: data => console.log(data),
  error: (error: HttpErrorResponse) => {
    console.log('Status:', error.status);
    console.log('Message:', error.message);
  }
});
```

------------------------------------------------------------------------

# Using catchError (Recommended)

``` ts
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

getUsers() {
  return this.http.get<User[]>('api/users').pipe(
    catchError((error: HttpErrorResponse) => {

      if (error.status === 401) {
        console.error('Unauthorized');
      } else if (error.status === 500) {
        console.error('Server Error');
      }

      return throwError(() => error);
    })
  );
}
```

------------------------------------------------------------------------

# Global Error Handling with Interceptor

``` ts
@Injectable()
export class ErrorInterceptor implements HttpInterceptor {

  intercept(req: HttpRequest<any>, next: HttpHandler) {

    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {

        if (error.status === 401) {
          console.error('Session Expired');
        }

        return throwError(() => error);
      })
    );
  }

}
```

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
