
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

### Step 2: Provide Token

``` ts
@NgModule({
  providers: [
    { provide: API_URL, useValue: 'https://api.production.com' }
  ]
})
export class AppModule {}
```

------------------------------------------------------------------------

### Step 3: Inject Token

``` ts
constructor(@Inject(API_URL) private apiUrl: string) {
  console.log(apiUrl);
}
```

Production Use Case:

-   Environment-based configuration
-   Feature toggles
-   Multi-tenant architecture
-   Clean separation of config from logic

------------------------------------------------------------------------

## Key Takeaways

-   Angular uses hierarchical injectors.
-   Services can have different scopes.
-   Constructor injection is default and most common.
-   InjectionToken is required for non-class dependencies.
-   Proper DI design improves scalability and testability.

