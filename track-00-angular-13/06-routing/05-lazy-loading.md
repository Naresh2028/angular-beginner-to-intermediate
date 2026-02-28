# Lazy Loading

## Definition

Lazy Loading is a routing technique where feature modules are loaded
only when needed.

Instead of loading the entire application at startup, Angular loads
modules on demand.

This improves performance and reduces initial bundle size.

------------------------------------------------------------------------

## Analogy

Think of an online streaming platform.

-   It does not download the entire movie library when you open the app.
-   It loads only the movie you select.

Lazy loading works the same way for feature modules.

------------------------------------------------------------------------

## Production-Level Example

### Step 1: Create Feature Module (AdminModule)

``` ts
@NgModule({
  declarations: [AdminComponent],
  imports: [CommonModule, RouterModule.forChild([
    { path: '', component: AdminComponent }
  ])]
})
export class AdminModule {}
```

------------------------------------------------------------------------

### Step 2: Configure Lazy Route

``` ts
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () =>
      import('./admin/admin.module').then(m => m.AdminModule)
  }
];
```

------------------------------------------------------------------------

How It Works:

-   User navigates to /admin
-   Angular loads AdminModule dynamically
-   Module is fetched only when required

------------------------------------------------------------------------

Production Benefits:

-   Faster initial load time
-   Better performance for large apps
-   Scalable architecture
-   Improved user experience

------------------------------------------------------------------------

## Key Takeaways

-   ActivatedRoute provides current route information.
-   Use snapshot for simple cases.
-   Use observables for dynamic parameter changes.
-   Lazy loading improves application performance.
-   Use loadChildren with dynamic imports.
-   Essential for enterprise-scale Angular applications.
