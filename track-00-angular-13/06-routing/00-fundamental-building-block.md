# Fundamental Building Blocks (Routes Array, RouterOutlet, RouterLink Directive)

## Definition

Angular Routing enables navigation between different views in a Single
Page Application (SPA).

The three fundamental building blocks are:

-   Routes Array → Defines path-to-component mapping
-   RouterOutlet → Placeholder where routed component loads
-   RouterLink → Directive used for navigation

These work together to enable dynamic navigation without full page
reload.

------------------------------------------------------------------------

## Analogy

Think of a railway system:

-   Routes Array → Railway map defining destinations
-   RouterOutlet → Platform where train arrives
-   RouterLink → Ticket that directs you to a destination

Navigation happens smoothly without rebuilding the entire station.

------------------------------------------------------------------------

# 1️⃣ Routes Array

## Definition

The Routes Array defines application navigation paths and maps them to
specific components.

------------------------------------------------------------------------

## Production-Level Example

### app-routing.module.ts

``` ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { DashboardComponent } from './dashboard/dashboard.component';
import { UsersComponent } from './users/users.component';

const routes: Routes = [
  { path: '', component: DashboardComponent },
  { path: 'users', component: UsersComponent },
  { path: '**', redirectTo: '' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

Production Use:

-   Feature module routing
-   Lazy loading
-   Guard-protected routes

------------------------------------------------------------------------

# 2️⃣ RouterOutlet

## Definition

RouterOutlet is a directive that acts as a placeholder for routed
components.

Angular loads the matched component into this location.

------------------------------------------------------------------------

## Production-Level Example

### app.component.html

``` html
<nav>
  <a routerLink="/">Dashboard</a>
  <a routerLink="/users">Users</a>
</nav>

<router-outlet></router-outlet>
```

When user clicks a link, the corresponding component renders inside
`<router-outlet>`.

------------------------------------------------------------------------

# 3️⃣ RouterLink Directive

## Definition

RouterLink is a directive used for navigation between routes.

It binds navigation paths to clickable elements.

------------------------------------------------------------------------

## Production-Level Example

``` html
<a routerLink="/users">Users</a>

<button [routerLink]="['/users']">
  Go to Users
</button>
```

Dynamic Navigation Example:

``` html
<a [routerLink]="['/users', user.id]">
  View User
</a>
```

Production Use:

-   Navigation menus
-   Dashboard navigation
-   Dynamic route parameters
-   Lazy-loaded module links

------------------------------------------------------------------------

# Complete Flow Example

1.  User clicks a link with `routerLink`
2.  Angular matches the path in Routes Array
3.  Router loads the component
4.  Component renders inside `router-outlet`

------------------------------------------------------------------------

## Key Takeaways

-   Routes Array defines navigation structure.
-   RouterOutlet renders matched components.
-   RouterLink triggers navigation.
-   Supports SPA behavior (no full page reload).
-   Foundation for advanced features like Guards, Lazy Loading, and
    Resolvers.
