# Fundamental Building Blocks

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

## Dynamic Routing

- Dynamic Routing is a technique where a single route can handle multiple values by using a parameterized path. 

- Instead of creating a separate route for every user (e.g., /user-1, /user-2), you define a placeholder like path: 'users/:id'.

- This allows the application to load the same component layout while dynamically fetching and displaying different data based on the ID provided in the URL.

## Define the Route with a Parameter

``` ts
  { path: 'users', component: UserlistComponent },
  { path: 'users/:id', component: UserdetailComponent }
```


## Component Implementation

### List Compoenent (Send data to the details Component) 

```ts
@Component({
  selector: 'app-userlist',
  template: `
    <ul>
      <li *ngFor="let user of users">
        <span>{{ user.name }}</span>
        <br />
        <a [routerLink]="['/users', user.id]">View Users</a>
      </li>
    </ul>
  `,
})
export class UserlistComponent implements OnInit {
  constructor() {}

  ngOnInit(): void {}

  users = [
    { id: 1, name: 'Naresh', role: '.NET Developer' },
    { id: 2, name: 'Sandy', role: 'QA' },
  ];
}
```

### Detail Component (Recieve id from user Component)

```ts
@Component({
  selector: 'app-userdetail',
  template: `
    <h2>User Details</h2>
    <p>Selected Id value is {{ userId }}</p>
    <button class="btn btn-primary" routerLink="/Users">Back to Users</button>
  `,
})
export class UserdetailComponent {
  userId!: string | null;

  constructor(private route: ActivatedRoute) {
    this.userId = this.route.snapshot.paramMap.get('id');
    console.log('Fetching data for user:', this.userId);
  }
}

```

### App-Component

```ts
@Component({
  selector: 'app-root',
  template: `
    <app-card>
      <h1 appHighlight class="card-title mb-3 text-primary">
        Angular Learning
      </h1>

      <app-userlist></app-userlist>
      <div class="container mt-4">
        <router-outlet></router-outlet>
      </div>
    </app-card>
  `,
})
export class AppComponent implements OnInit {}
```

### Output

<img width="571" height="515" alt="image" src="https://github.com/user-attachments/assets/d4e990a6-19db-4da3-8540-59557630c286" />

Path Variable Capture: The message "Selected Id value is 2" proves that your UserDetailComponent successfully used ActivatedRoute to extract the ID from the URL.

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
