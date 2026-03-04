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

``` ts
import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {

    const token = localStorage.getItem('authToken');

    const modifiedReq = req.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`
      }
    });

    return next.handle(modifiedReq);
  }

}
```

------------------------------------------------------------------------

### Step 2: Provide Interceptor

``` ts
import { HTTP_INTERCEPTORS } from '@angular/common/http';

@NgModule({
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ]
})
export class AppModule {}
```

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
