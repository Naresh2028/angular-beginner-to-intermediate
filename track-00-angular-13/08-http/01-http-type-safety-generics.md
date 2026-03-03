# HTTP Type Safety and Generics

## Definition

HTTP Type Safety in Angular ensures that the data returned from an API
is strongly typed using TypeScript interfaces and generics.

Angular's HttpClient supports generics to define the expected response
type.

Syntax:

``` ts
this.http.get<Type>(url)
```

This improves:

-   Compile-time validation
-   Autocomplete support
-   Reduced runtime errors
-   Better maintainability

------------------------------------------------------------------------

## Why Type Safety Matters

Without generics:

``` ts
this.http.get('api/users')
```

Return type becomes `Observable<Object>`.

With generics:

``` ts
this.http.get<User[]>('api/users')
```

Return type becomes `Observable<User[]>`.

This ensures strict typing across the application.

------------------------------------------------------------------------

# Step 1️⃣ Define Interface (Model)

``` ts
export interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}
```

------------------------------------------------------------------------

# Step 2️⃣ Use Generics in Service

``` ts
@Injectable({ providedIn: 'root' })
export class UserService {

  constructor(private http: HttpClient) {}

  getUsers() {
    return this.http.get<User[]>('https://api.example.com/users');
  }

  getUserById(id: number) {
    return this.http.get<User>(
      `https://api.example.com/users/${id}`
    );
  }

}
```

------------------------------------------------------------------------

# Step 3️⃣ Component Usage

``` ts
export class UsersComponent {

  users: User[] = [];

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userService.getUsers().subscribe(data => {
      this.users = data;
    });
  }

}
```

Now TypeScript knows:

-   `users` is an array of User
-   Accessing `users[0].name` is type-safe
-   Invalid properties cause compile-time errors

------------------------------------------------------------------------

# Production-Level Example (Generic API Wrapper)

### Create Generic API Response Interface

``` ts
export interface ApiResponse<T> {
  success: boolean;
  message: string;
  data: T;
}
```

------------------------------------------------------------------------

### Use Generic Wrapper

``` ts
export class UserService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private url: string,
  ) {}

  getUsers(): Observable<ApiResponse<User[]>> {
    return this.http.get<ApiResponse<User[]>>(this.url);
  }
}
```

------------------------------------------------------------------------

### Access Data Safely

``` ts
@Component({
  selector: 'app-user',
  template: `
    <div *ngIf="errorMessage" class="alert alert-danger">
      {{ errorMessage }}
    </div>

    <ul class="list-group" *ngIf="users.length > 0">
      <li class="list-group-item" *ngFor="let user of users">
        {{ user.name }} ({{ user.email }})
      </li>
    </ul>
  `,
})
export class UserComponent implements OnInit {
  users: User[] = [];
  errorMessage: string = '';

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    this.userService.getUsers().subscribe({
      next: (response) => {
        if (response.success) {
          this.users = response.data;
        } else {
          this.errorMessage = response.message;
        }
      },
      error: (err) => {
        this.errorMessage = 'Network Error: ' + err.message;
      },
    });
  }
}
```

This ensures both structure and nested data are strongly typed.

------------------------------------------------------------------------

# Advanced Example (Reusable Generic Service)

``` ts
@Injectable({ providedIn: 'root' })
export class ApiService {

  constructor(private http: HttpClient) {}

  get<T>(url: string) {
    return this.http.get<T>(url);
  }

  post<T>(url: string, body: any) {
    return this.http.post<T>(url, body);
  }

}
```

Usage:

``` ts
this.apiService.get<User[]>('api/users')
  .subscribe(users => console.log(users));
```

Highly reusable and scalable.

------------------------------------------------------------------------

# Best Practices

-   Always define interfaces for API responses.
-   Avoid using `any`.
-   Use generic wrappers for standard API response structures.
-   Separate models into dedicated files.
-   Combine with RxJS operators for better error handling.

------------------------------------------------------------------------

## Key Takeaways

-   Generics enable strong typing in HTTP calls.
-   Improves IntelliSense and compile-time safety.
-   Prevents accidental property access errors.
-   Essential for enterprise Angular applications.
-   Promotes scalable and maintainable code.
