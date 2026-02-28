# Route Guards

## Definition

Route Guards are services that control navigation to or from a route.

They decide whether a route should be:

-   Activated (CanActivate)
-   Deactivated (CanDeactivate)
-   Loaded (CanLoad)
-   Matched (CanMatch)

Guards improve security and user flow control.

------------------------------------------------------------------------

## Analogy

Think of airport security.

-   Before boarding a flight, your credentials are verified.
-   If you don't meet requirements, access is denied.

Route Guards protect routes the same way.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Protect Admin Route

### Step 1: Create Guard

``` ts
import { Injectable } from '@angular/core';
import { CanActivate } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  isLoggedIn = false; // Example condition

  canActivate(): boolean {
    return this.isLoggedIn;
  }

}
```

------------------------------------------------------------------------

### Step 2: Apply Guard in Routing

``` ts
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [AuthGuard]
  }
];
```

If `isLoggedIn` is false → Navigation is blocked.

------------------------------------------------------------------------

Production Use Cases:

-   Authentication & Authorization
-   Role-based access control
-   Prevent leaving unsaved forms (CanDeactivate)
-   Lazy-loaded module protection

------------------------------------------------------------------------

## Key Takeaways

-   Nested routes create hierarchical navigation.
-   Parent route must contain `<router-outlet>`{=html}.
-   Guards control route access and navigation flow.
-   Essential for security in enterprise apps.
-   Keep guard logic lightweight and delegate heavy logic to services.
