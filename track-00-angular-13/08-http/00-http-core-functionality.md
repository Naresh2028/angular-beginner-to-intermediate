# Core Functionality: Performing HTTP Requests

## Definition

Angular performs HTTP requests using the HttpClient service from
`@angular/common/http`.

HttpClient allows communication with backend APIs to:

-   Fetch data (GET)
-   Create data (POST)
-   Replace data (PUT)
-   Partially update data (PATCH)
-   Delete data (DELETE)

It returns Observables, making it reactive and asynchronous.

------------------------------------------------------------------------

## Setup

### 1️⃣ Import HttpClientModule

``` ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule {}
```

------------------------------------------------------------------------

# 1️⃣ GET Request (Read Data)

## Use Case

Fetching users from API.

``` ts
@Injectable({ providedIn: 'root' })
export class UserService {

  constructor(private http: HttpClient) {}

  getUsers() {
    return this.http.get<any[]>('https://api.example.com/users');
  }

}
```

Component:

``` ts
this.userService.getUsers().subscribe(data => {
  this.users = data;
});
```

------------------------------------------------------------------------

# 2️⃣ POST Request (Create Data)

## Use Case

Creating a new user.

``` ts
createUser(user: any) {
  return this.http.post('https://api.example.com/users', user);
}
```

Component:

``` ts
this.userService.createUser(this.form.value)
  .subscribe(response => {
    console.log('User Created', response);
  });
```

------------------------------------------------------------------------

# 3️⃣ PUT Request (Replace Data)

## Use Case

Updating entire user record.

``` ts
updateUser(id: number, user: any) {
  return this.http.put(`https://api.example.com/users/${id}`, user);
}
```

------------------------------------------------------------------------

# 4️⃣ PATCH Request (Partial Update)

## Use Case

Updating only email.

``` ts
updateUserEmail(id: number, email: string) {
  return this.http.patch(
    `https://api.example.com/users/${id}`,
    { email }
  );
}
```

------------------------------------------------------------------------

# 5️⃣ DELETE Request (Remove Data)

## Use Case

Deleting user.

``` ts
deleteUser(id: number) {
  return this.http.delete(
    `https://api.example.com/users/${id}`
  );
}
```

------------------------------------------------------------------------

# Production-Level Service Example

``` ts
@Injectable({ providedIn: 'root' })
export class ProductService {

  private apiUrl = 'https://api.example.com/products';

  constructor(private http: HttpClient) {}

  getProducts() {
    return this.http.get<any[]>(this.apiUrl);
  }

  addProduct(product: any) {
    return this.http.post(this.apiUrl, product);
  }

  updateProduct(id: number, product: any) {
    return this.http.put(`${this.apiUrl}/${id}`, product);
  }

  deleteProduct(id: number) {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }

}
```

------------------------------------------------------------------------

# Best Practices (Production)

-   Always use services to handle HTTP logic.
-   Use strong typing instead of `any`.
-   Handle errors using `catchError`.
-   Use interceptors for auth tokens.
-   Avoid subscribing inside services.
-   Prefer async pipe when possible.

------------------------------------------------------------------------

## Key Takeaways

-   HttpClient handles all HTTP methods.
-   Returns Observables (reactive pattern).
-   Separate API logic into services.
-   Follow REST principles.
-   Essential for full-stack Angular applications.
