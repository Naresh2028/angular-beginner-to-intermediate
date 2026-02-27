# Route Matching and Types

## Definition

Route Matching in Angular determines how the router selects a route
configuration when navigating to a URL.

Angular follows the **First-Match Wins Strategy** --- it checks routes
from top to bottom and selects the first matching route.

Route Types:

-   Static Routes
-   Dynamic Routes
-   Wildcard Routes

Order of routes is critical for correct behavior.

------------------------------------------------------------------------

## Analogy

Think of a customer support system:

-   Requests are checked in order.
-   The first department that matches the issue handles it.
-   If nothing matches, the request goes to a fallback department.

Angular routing works the same way.

------------------------------------------------------------------------

# First-Match Wins Strategy

## Definition

Angular evaluates routes in the order they are defined. The first route
that matches the URL is selected.

------------------------------------------------------------------------

## Production-Level Example

``` ts
const routes: Routes = [
  { path: 'users/new', component: NewUserComponent },
  { path: 'users/:id', component: UserDetailComponent }
];
```

If the order is reversed:

``` ts
{ path: 'users/:id', component: UserDetailComponent },
{ path: 'users/new', component: NewUserComponent }
```

Then navigating to `/users/new` will incorrectly match `:id`.

Correct ordering prevents routing bugs.

------------------------------------------------------------------------

# Static Routes

## Definition

Static routes have fixed paths without parameters.

------------------------------------------------------------------------

## Production-Level Example

``` ts
const routes: Routes = [
  { path: '', component: DashboardComponent },
  { path: 'login', component: LoginComponent },
  { path: 'about', component: AboutComponent }
];
```

Use Cases:

-   Landing pages
-   Login pages
-   Static dashboards

------------------------------------------------------------------------

# Dynamic Routes

## Definition

Dynamic routes use route parameters to capture variable values.

Syntax:

``` ts
path: 'users/:id'
```

------------------------------------------------------------------------

## Production-Level Example

``` ts
const routes: Routes = [
  { path: 'users/:id', component: UserDetailComponent }
];
```

Access parameter in component:

``` ts
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) {}

ngOnInit() {
  const id = this.route.snapshot.paramMap.get('id');
  console.log(id);
}
```

Production Use Cases:

-   Product detail pages
-   User profiles
-   Order tracking pages

------------------------------------------------------------------------

# Wildcard Routes

## Definition

Wildcard route (`**`) catches all unmatched URLs.

It should always be placed last in the routes array.

------------------------------------------------------------------------

## Production-Level Example

``` ts
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'products', component: ProductsComponent },
  { path: '**', component: NotFoundComponent }
];
```

Production Use Cases:

-   404 Not Found pages
-   Fallback redirects
-   Error handling routes

------------------------------------------------------------------------

# Complete Example

``` ts
const routes: Routes = [
  { path: '', component: DashboardComponent },
  { path: 'users/new', component: NewUserComponent },
  { path: 'users/:id', component: UserDetailComponent },
  { path: 'login', component: LoginComponent },
  { path: '**', redirectTo: '' }
];
```

------------------------------------------------------------------------

## Key Takeaways

-   Route order matters (First-Match Wins).
-   Static routes handle fixed paths.
-   Dynamic routes handle parameterized paths.
-   Wildcard routes handle unmatched URLs.
-   Always place wildcard routes at the end.
-   Proper route structure prevents unexpected navigation behavior.
