
# NEW FEATURES (Angular 15)

## List the Features

Angular 15 focused heavily on simplifying Angular architecture and making standalone APIs production-ready.

Major features include:

1. Stable Standalone Components
2. Standalone Router APIs
3. Standalone HTTP APIs
4. Directive Composition API
5. Improved Image Directive (NgOptimizedImage)
6. Functional Route Guards
7. ESBuild-based Build Improvements
8. Improved Angular CLI

---

# 1. Stable Standalone Components

## Example

In Angular 15, components, directives, and pipes can be marked as standalone: true. They manage their own dependencies via the imports array.

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-demo',
  standalone: true,
  template: `<h1>Hello Angular 15</h1>`
})
export class DemoComponent {}
```

## Key Notes

- Removes the need for NgModules
- Simplifies application structure
- Makes Angular architecture cleaner

---

# 2. Standalone Router APIs

## Example

Instead of RouterModule.forRoot() and HttpClientModule, Angular 15 introduced Provide Functions. These are used in your main.ts to configure the application during bootstrap.

```ts
import { provideRouter, Router, Routes } from '@angular/router';
import { DataComponent } from './Components/data/data.component';
import { provideHttpClient } from '@angular/common/http';
import { ApplicationConfig } from '@angular/platform-browser';
import { HomeComponent } from './Components/home/home.component';

const routes: Routes = [
  { path: 'data', component: DataComponent },
  { path: '', component: HomeComponent, pathMatch: 'full' },
];

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)],
};
```

## Key Notes

- Simplifies routing setup
- Works perfectly with standalone components
- Reduces boilerplate code

---

# 3. Standalone HTTP APIs

## Example

Angular 15 allows HTTP configuration without NgModules.

#### @app.config.te
```ts

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient()
]
};

```

---

Here is how you consume the HTTP service inside a standalone component.

````ts
@Component({
  selector: 'app-data',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './data.component.html',
  styleUrls: ['./data.component.css'],
})
export class DataComponent implements OnInit {
  post$!: Observable<Post>;

  private http = inject(HttpClient);

  ngOnInit(): void {
    this.post$ = this.http.get<Post>('https://jsonplaceholder.typicode.com/posts/1');
  }
}

````

## Key Notes

- Eliminates the need for HttpClientModule
- Simplifies dependency injection
- Better integration with standalone architecture.

### Output

<img width="1319" height="357" alt="image" src="https://github.com/user-attachments/assets/9cb157dd-9752-4419-8544-c3958522fdfc" />


---

# 4. Directive Composition API

The Directive Composition API, introduced in Angular 15, is one of the most powerful features for code reuse.

## What it replaces

1. Class Inheritance: You could extend a base class. However, in TypeScript, a class can only inherit from one parent. This made it impossible to share multiple independent behaviors.

2. Manual Application: You had to manually add every directive to the HTML selector in every template (e.g., <my-comp appHover appLog appToolip>). This was repetitive and error-prone.

## Example (Create the Reusable Directive)

First, we create a standalone directive that adds a border and a "Secret" prefix to any text.

```ts
import { Directive, HostBinding } from '@angular/core';

@Directive({
  selector: '[appSecret]',
  standalone: true,
})
export class SecretDirective {
  @HostBinding('style.border') border = '2px dashed red';
  @HostBinding('style.padding') padding = '10px';
}

```

## 2. Compose it in a Component

We apply the directive inside the decorator. The user of this component doesn't even need to know the directive exists!

````ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { SecretDirective } from 'src/app/Directives/secret.directive';

@Component({
  selector: 'app-card',
  standalone: true,
  imports: [CommonModule],
  template: `<ng-content></ng-content>`,
  styleUrls: ['./card.component.css'],
  hostDirectives: [SecretDirective],
})
export class CardComponent {}

````


---

## 3. Usage in App

````ts
@Component({
  selector: 'app-root',
  template: `
    <h2>Angular 15 standalone Project Demo</h2>

    <app-card> This content is automatically added & padded </app-card>

    <router-outlet></router-outlet>
  `,
  standalone: true,
  imports: [RouterOutlet, CardComponent],
})
export class AppComponent {
  title = 'angular-15-app';
}
````



Directive composition enables combining multiple directive behaviors.

### Ouput

<img width="515" height="416" alt="image" src="https://github.com/user-attachments/assets/10f4bb15-a5f9-49a8-916f-e57b6ce0ba6d" />


## Key Notes

- Encourages code reuse
- Improves component composition
- Reduces duplicated logic

---

# 5. NgOptimizedImage Directive

NgOptimizedImage is a standalone directive (using the ngSrc attribute) that optimizes image loading. It prevents common performance issues like Layout Shift (CLS) and slow Largest Contentful Paint (LCP) by enforcing attributes like aspect ratios and modern loading strategies.

## Why it was born (What it replaces)

Before this directive, developers used the standard HTML <img> tag. This often led to poor performance because:

- Missing Dimensions: Forgetting width and height caused the page to "jump" as images loaded (Layout Shift).

- LCP Issues: Important images (like banners) weren't prioritized, making the site feel slow.


````ts
@Component({
  selector: 'app-card',
  standalone: true,
  imports: [CommonModule, NgOptimizedImage],
  template: `
    <img ngSrc="assets/Images/logo.png" width="225" height="225" priority />
  `,
  styleUrls: ['./card.component.css'],
  hostDirectives: [SecretDirective],
})
export class CardComponent {}
````


## Keynotes

- LCP Protection: If an image is identified as the Largest Contentful Paint but doesn't have the priority attribute, Angular will throw a warning in the console to help you fix it.

<img width="1920" height="453" alt="image" src="https://github.com/user-attachments/assets/20ec04be-ac19-4e28-b358-dd1412eadd64" />


- Aspect Ratio: It forces you to define width and height (or use fill mode), which completely eliminates image-related Cumulative Layout Shift (CLS).
<img width="1920" height="466" alt="image" src="https://github.com/user-attachments/assets/100d722c-8703-412e-ac7f-1f5b0c6e4ea4" />

---

# 6. Functional Route Guards

## Definition
A Functional Route Guard is a plain JavaScript/TypeScript function that returns a boolean, a UrlTree, or an Observable/Promise of either. It uses the inject() function to access services like AuthService or Router directly inside the function body, rather than through a class constructor.

## Why it was born

Before Angular 15, route guards had to be Class-based. This required:

- Implementing an interface (like CanActivate).

- Declaring a class with @Injectable.

- Injecting dependencies via a formal constructor.

## Example

### 1. Generate the Guard

Use the CLI to generate a functional guard. It will ask you which type (e.g., canActivate) you want.

````ts
ng generate guard auth --functional
````

### 2. The Functional Logic

Instead of a class, you get a clean function. We use inject to get our AuthService.


```ts
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from '../Services/auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isHeLogged()) {
    return true;
  } else {
    return router.parseUrl('/login');
  }
};

```

### 3. Application in Routes

You can now pass the function directly into the canActivate array.

```ts
const routes: Routes = [
  { path: 'data', component: DataComponent },
  {path:'login',component:LoginComponent},
  {path:'dashboard',component:DashboardComponent,canActivate:[authGuard]},
];
```

### Output

<img width="867" height="246" alt="image" src="https://github.com/user-attachments/assets/9d2badae-bd48-4ae9-aca7-01f7a6afbef9" />


## Key Notes

- Simplifies guard creation

- Boilerplate Reduction: No more @Injectable or constructor.

- The inject() Power: You can only use inject() during the "Injection Context" (like when the guard is first called by the router).

---

# 7. ESBuild-based Build Improvements

## Example

Angular CLI introduced faster build pipelines using ESBuild internally.

## Key Notes

- Faster builds
- Improved development performance
- Reduced compilation time

---

# 8. Improved Angular CLI

## Example

Angular CLI improvements include:

- Better error messages
- Faster dependency resolution
- Simplified configuration

## Key Notes

- Improved developer experience
- Faster builds and serving
- Better project maintenance

---

# Summary

Angular 15 focused on simplifying Angular development and making standalone APIs the future of Angular architecture.

Major highlights:

- Stable Standalone Components
- Standalone Router APIs
- Standalone HTTP APIs
- Directive Composition API
- Improved performance and build speed
