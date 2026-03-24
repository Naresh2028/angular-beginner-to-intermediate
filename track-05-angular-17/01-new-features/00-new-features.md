# NEW FEATURES (Angular 17)

## List the Features

Angular 17 focuses on **simplifying templates, improving performance, and enhancing developer experience**.

Key features:

1. New Control Flow Syntax (@if, @for, @switch) (Developer preview)
2. Deferrable Views (@defer)
3. Hydration (Production Ready)
4. Standalone by Default (ng new my-app)
5. Improved Build & Runtime Performance (SSG, & SSR 87%)
6. esbuild and Vite-based Dev Server enabled by default
7. Continued Signals Integration
8. Custom Input Transforms 

---

# 1. NEW CONTROL FLOW

In Angular 17, the framework introduced a Built-in Control Flow that moves conditional logic and loops directly into the compiler. While it launched in "Developer Preview" in v17.

### Defination

Built-in Control Flow is a new, block-based syntax (@if, @for, @switch) used in Angular templates to handle dynamic rendering. Unlike the older structural directives (*ngIf, *ngFor) which are based on HTML attributes and require CommonModule imports.

It is "built-in," meaning it is available in every component automatically without any extra configuration or imports.


## @if vs *ngIf Example

### Older Way (v16 and below):

You had to use a separate <ng-template> and a reference variable to handle the "else" case.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div *ngIf="isLoaded; else loading">
      <p>Data is here!</p>
    </div>

    <ng-template #loading>
      <p>Loading...</p>
    </ng-template>
  `,
})
export class ControlFlowComponent {
  isLoaded = false;

  constructor() {
    setTimeout(() => (this.isLoaded = true), 2000);
  }
}
````


### New way

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `

    @if (isLoaded) {
      <p>Data is here</p>
    } @else {
      <p>Loading...</p>
    }

  `,
})
export class ControlFlowComponent {
  isLoaded = false;

  constructor() {
    setTimeout(() => (this.isLoaded = true), 2000);
  }
}
````

- The old way was visually noisy and required managing "template references" (#loading) which are hard to track in large files.











