
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

## Example

Angular introduced an optimized image directive for performance.

```html
<img ngSrc="logo.png" width="200" height="100">
```

## Key Notes

- Automatic lazy loading
- Image performance optimization
- Improves Largest Contentful Paint (LCP)

---

# 6. Functional Route Guards

## Example

Angular 15 supports functional route guards.

```ts
export const authGuard = () => {
  return true;
};
```

Usage:

```ts
{
  path: 'dashboard',
  canActivate: [authGuard]
}
```

## Key Notes

- Simplifies guard creation
- Removes need for class-based guards
- Works well with standalone APIs

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
