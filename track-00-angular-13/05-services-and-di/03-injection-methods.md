
# Injection Methods

# Constructor Injection

## Definition

Constructor Injection is the most common way to inject dependencies.

Angular automatically resolves dependencies defined in the constructor.

------------------------------------------------------------------------

## Analogy

Think of ordering food at a restaurant table.

You sit down (component created). The waiter brings everything you
ordered (dependencies injected).

------------------------------------------------------------------------

## Production-Level Example

``` ts
@Component({
  selector: 'app-orders',
  template: `<p>Orders</p>`
})
export class OrdersComponent {

  constructor(private orderService: OrderService) {}

}
```

Angular automatically injects OrderService.

------------------------------------------------------------------------

# Injection Token

## Definition

Injection Token is used when injecting non-class dependencies such as:

-   Configuration values
-   Strings
-   Interfaces
-   API endpoints

It allows Angular to uniquely identify dependencies.

------------------------------------------------------------------------

## Analogy

Think of a locker key.

The key (InjectionToken) identifies which locker (dependency) to open.

------------------------------------------------------------------------

## Production-Level Example

### Step 1: Create Token

``` ts
import { InjectionToken } from '@angular/core';

export const API_URL = new InjectionToken<string>('API_URL');

```

------------------------------------------------------------------------

### Step 2: Provide or Register the Token

``` ts
@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    GameModule,
    FormsModule,
    HttpClientModule,
    FeatureModule
],
  providers: [
    {provide:API_URL,useValue:'https://jsonplaceholder.typicode.com/posts'}
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

------------------------------------------------------------------------

### Step 3: Inject Token

``` ts
import { Injectable, Inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Post } from 'src/app/Interface/post';
import { API_URL } from 'src/app/Class/token';

@Injectable({
  providedIn: 'root',
})
export class PostService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private api: string,
  ) {}

  getPost(): Observable<Post[]> {
    return this.http.get<Post[]>(this.api);
  }
}

```

## Key Points to Remember

- @Inject Decorator: This is mandatory for tokens. It tells Angular: "Don't look for a class named apiUrl, look for the value registered under the API_URL token".

- Type Safety: By defining new InjectionToken<string>, TypeScript will ensure that whatever you provide in the useValue field is actually a string.

Production Use Case:

-   Environment-based url configuration
-   Multi-tenant architecture
-   Clean separation of config from logic

------------------------------------------------------------------------

## Key Takeaways

-   Angular uses hierarchical injectors.
-   Services can have different scopes.
-   Constructor injection is default and most common.
-   InjectionToken is required for non-class dependencies.
-   Proper DI design improves scalability and testability.

