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

In Angular, the Router parses the routes array from top to bottom. It stops as soon as it finds the first match. If you place a broad route before a specific one, the specific one will never be reached.

------------------------------------------------------------------------

### ❌ Incorrect Example (Broken Strategy)

If we moved your wildcard to the top, your entire application would break because ** matches everything.

``` ts
const routes: Routes = [
  { path: '**', component: NotFoundComponentComponent }, // 🛑 FIRST MATCH! Every URL will redirect to Home.
  //{path:'',component:HomeComponent},
  { path: 'dashboard', component: DashboardComponent },
  { path: 'settings', component: SettingsComponent },
  { path: 'profile', component: ProfileComponent },
  { path: 'logout', component: LogoutComponent },
  { path: 'users', component: UserlistComponent },
  { path: 'users/:id', component: UserdetailComponent }
];
````

#### Output

<img width="1401" height="521" alt="image" src="https://github.com/user-attachments/assets/8b26159a-2aff-4b8d-8a9a-b5a57bf24be8" />

- Whatever the url is changes, it will always remains same.


### Correct Comfiguration

```ts
const routes: Routes = [
  // 1. Exact match for empty URL
  {path:'',component:HomeComponent, pathMatch:'full'},

  // 2. Exact static matches
  { path: 'dashboard', component: DashboardComponent },
  { path: 'settings', component: SettingsComponent },
  { path: 'profile', component: ProfileComponent },
  { path: 'logout', component: LogoutComponent },

  // 3. Specific list match
  { path: 'users', component: UserlistComponent },

  // 4. Dynamic match (matches 'users/1' but NOT 'users')
  { path: 'users/:id', component: UserdetailComponent },

  // 5. The "Safety Net"
  { path: '**', component: NotFoundComponentComponent },
];
````


Correct ordering prevents routing bugs in earlier stage.

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
  {path:'',component:HomeComponent, pathMatch:'full'},
  { path: 'dashboard', component: DashboardComponent },
  { path: 'settings', component: SettingsComponent },
  { path: 'profile', component: ProfileComponent },
  { path: 'logout', component: LogoutComponent },
  { path: 'users', component: UserlistComponent },
  { path: 'users/:id', component: UserdetailComponent },
  { path: '**', component: NotFoundComponentComponent },
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
