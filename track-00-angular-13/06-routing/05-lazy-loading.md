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
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ComponentAComponent } from './component-a/component-a.component';
import { ComponentBComponent } from './component-b/component-b.component';
import { FeatureService } from 'src/app/Services/feature/feature.service';
import { featureRoutingModule } from './feature-routing.module';

@NgModule({
  declarations: [ComponentAComponent, ComponentBComponent],
  imports: [CommonModule, featureRoutingModule],
  exports: [ComponentAComponent, ComponentBComponent],
  providers: [FeatureService],
})
export class FeatureModule {}
```

------------------------------------------------------------------------

### Child Route Configuration

You can create a feature module without Angular cli command.

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ComponentAComponent } from './component-a/component-a.component';
import { ComponentBComponent } from './component-b/component-b.component';

const routes: Routes = [
  { path: 'a', component: ComponentAComponent },
  { path: 'b', component: ComponentBComponent },
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class featureRoutingModule {}

```

### Import Feature Route class file in Feature Module

````ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ComponentAComponent } from './component-a/component-a.component';
import { ComponentBComponent } from './component-b/component-b.component';
import { FeatureService } from 'src/app/Services/feature/feature.service';
import { featureRoutingModule } from './feature-routing.module';

@NgModule({
  declarations: [ComponentAComponent, ComponentBComponent],
  imports: [CommonModule, featureRoutingModule],
  exports: [ComponentAComponent, ComponentBComponent],
  providers: [FeatureService],
})
export class FeatureModule {}

````

------------------------------------------------------------------------

### Configure Lazy Route

``` ts
const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () =>
      import('./feature/feature/feature.module').then((m) => m.FeatureModule),
  },
  { path: 'settings', component: SettingsComponent },
  { path: 'profile', component: ProfileComponent },
  { path: 'logout', component: LogoutComponent },
];
```

### Output

<img width="1357" height="988" alt="image" src="https://github.com/user-attachments/assets/d6a5a6a7-ff6f-4637-b4e1-273e59bdfb68" />



### What Happens if you forgot to remove the feature module from the app.module.ts

````ts
@NgModule({
  declarations: [
    AppComponent,
    PlayerComponent,
    LifecycleDemoComponent,
    WrapperComponent,
    ViewComponentComponent,
    DestroyComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    GameModule,
    FormsModule,
    HttpClientModule,
    FeatureModule, // Don't forgot to remove it if you configured as childRoute.
    ReactiveFormsModule
],
  providers: [
    {provide:API_URL,useValue:'https://jsonplaceholder.typicode.com/posts'}
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

````

<img width="1332" height="495" alt="image" src="https://github.com/user-attachments/assets/0b84dd1c-b043-45e0-b193-a8f3291b21b3" />


------------------------------------------------------------------------

Key Takeaways for Lazy Loading:

- No AppModule Import: Ensure FeatureModule is not listed in the imports array of app.module.ts. If it is imported there, it will be loaded eagerly, defeating the purpose.
  
- Module Chunking: When you run ng build, you will notice a separate .js file (a chunk) created specifically for your FeatureModule.

<img width="1379" height="340" alt="image" src="https://github.com/user-attachments/assets/e5109281-b9a4-47a2-a956-7d39dd00d904" />

- Pathing: Navigating to localhost:4200/feature/a will now trigger the download of the module and display ComponentAComponent.

- Service Scope: Since your FeatureService is provided in the FeatureModule, a single instance of that service will be created only when the module is loaded.

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
