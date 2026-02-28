# Nested (Child) Routes

## Definition

Nested Routes (Child Routes) allow you to define routes inside other
routes.

They enable hierarchical navigation where a parent component contains a
`<router-outlet>`{=html} for rendering its child components.

This is useful for building complex layouts like dashboards with
multiple sections.

------------------------------------------------------------------------

## Analogy

Think of a shopping mall.

-   The mall is the parent route.
-   Individual stores inside the mall are child routes.
-   You enter the mall first, then navigate to specific stores.

Parent layout stays constant, inner content changes.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Admin Dashboard with Sections

### Routing Configuration

``` ts
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    children: [
      { path: 'users', component: AdminUsersComponent },
      { path: 'reports', component: AdminReportsComponent },
      { path: '', redirectTo: 'users', pathMatch: 'full' }
    ]
  }
];
```

------------------------------------------------------------------------

### Parent Template (admin.component.html)

``` html
<h2>Admin Panel</h2>

<nav>
  <a routerLink="users">Users</a>
  <a routerLink="reports">Reports</a>
</nav>

<router-outlet></router-outlet>
```

------------------------------------------------------------------------

Production Use Cases:

-   Admin dashboards
-   Settings pages with tabs
-   Multi-step workflows
-   Feature module routing

------------------------------------------------------------------------

