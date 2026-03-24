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


## 1. Conditional Rendering (*ngIf vs @if)

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

**Reason To Change**

- The old way was visually noisy and required managing "template references" (#loading) which are hard to track in large files.

---

## 2. Looping (*ngFor vs @for)

Old Way:
Manual trackBy function in the .ts file was mandatory for performance but often ignored.


````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [NgForOf, NgIf],  // Import Required
  template: `
    <div *ngIf="Users.length > 0">
      <div *ngFor="let user of Users">
        <p>{{ user.userName }}</p>
      </div>
    </div>

    <div *ngIf="Users.length === 0">
      <p>User data is Empty</p>
    </div>
  `,
})
export class ControlFlowComponent {
  Users = [
    { id: 1, userName: 'Naresh', isWorking: true },
    { id: 2, userName: 'alien', isWorking: false },
    { id: 3, userName: 'Larthick', isWorking: true },
  ];

  constructor() {}
}
````

### New Way

The track property is now mandatory, and there is a built-in block for empty lists.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `

  <ul>
    @for (item of Users; track item.id) {
      <li>{{item.userName}}</li>
    } @empty {
      <li>No items to display...</li>
    }
  </ul>
  `,
})
export class ControlFlowComponent {
  Users = [
    { id: 1, userName: 'Naresh', isWorking: true },
    { id: 2, userName: 'alien', isWorking: false },
    { id: 3, userName: 'Larthick', isWorking: true },
  ];

  constructor() {}
}
````
 
**Reason to Change**

- By making track mandatory, Angular prevents common performance bugs where the entire list re-renders unnecessarily. 

- The @empty block also removes the need for a separate *ngIf="items.length === 0" check.


---

## 3. Switching: @switch vs *ngSwitch

### Older Way (v16 and below):

Required an attribute on a container and then specific directives on each child element.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [NgSwitch, NgSwitchCase, NgSwitchDefault],
  template: `
    <div [ngSwitch]="UserRole">
      <div *ngSwitchCase="'admin'">
        <p>Welcome Admin..</p>
      </div>
      <div *ngSwitchCase="'user'">
        <p>Welcome User..</p>
      </div>
      <div *ngSwitchDefault>
        <p>Welcome Guest...</p>
      </div>
    </div>
  `,
})
export class ControlFlowComponent {
  UserRole = ['admin', 'user'];
}
````

### New Way (v17+):

this is now a "pure" standalone component with zero external dependencies for its logic.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `
    @switch (UserRole) {
      @case ('admin') {<p>Welcome Admin!</p>}
      @case ('user') {<p>Welcome User!</p>}
      @default {
        <p>Welcome Guest!</p>
      }
    }
  `,
})
export class ControlFlowComponent {
  UserRole: 'admin' | 'user' | 'guest' = 'guest';
}
````

**Reason to Change**

- The old *ngSwitch was technically a set of separate directives working together, which was slower to process. 

- The new @switch is a single block of logic, making it significantly faster for the browser to execute.

### Key Take Aways

- Performance: Up to 90% faster execution for complex loops and conditionals.

- Bundle Size: Because these are built into the compiler, they don't require the extra code associated with the NgIf and NgFor classes.










