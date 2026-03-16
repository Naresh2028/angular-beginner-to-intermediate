
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

Standalone components became **fully stable** in Angular 15.

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

Angular 15 allows router configuration without modules.

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { provideRouter } from '@angular/router';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes)
  ]
});
```

## Key Notes

- Simplifies routing setup
- Works perfectly with standalone components
- Reduces boilerplate code

---

# 3. Standalone HTTP APIs

## Example

Angular 15 allows HTTP configuration without NgModules.

```ts
import { provideHttpClient } from '@angular/common/http';

bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient()
  ]
});
```

## Key Notes

- Eliminates the need for HttpClientModule
- Simplifies dependency injection
- Better integration with standalone architecture

---

# 4. Directive Composition API

## Example

Angular 15 allows reusing directive behavior across components.

```ts
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {}
```

Directive composition enables combining multiple directive behaviors.

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
