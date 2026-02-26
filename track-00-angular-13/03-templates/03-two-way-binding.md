# Two-way Data Binding

## Definition

Two-way data binding in Angular allows synchronization between the
component class and the template.

It combines:

-   Property Binding → `[value]`
-   Event Binding → `(input)`

Angular provides shorthand syntax using `[(ngModel)]`.

Syntax:

``` html
[(ngModel)]="property"
```

It keeps the model and the view in sync automatically.

------------------------------------------------------------------------

## Analogy

Think of a shared Google Document.

-   When you type (view changes),
-   The document updates instantly (model updates).
-   When someone else edits (model changes),
-   Your screen updates immediately (view updates).

Two-way binding works exactly like that --- both sides stay
synchronized.

------------------------------------------------------------------------

## Two Production-Level Examples

### 1️⃣ Real-Time Form Input (User Profile)

#### Component

``` ts
import {
  AfterViewChecked,
  AfterViewInit,
  Component,
  OnDestroy,
  OnInit,
  ViewChild,
} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 class="card-title mb-3 text-primary">Angular Learning</h1>

      <input type="text" class="form-control" [(ngModel)]="userName" />
      <p>{{ userName }}</p>
    </app-card>
  `,
})
export class AppComponent {
  userName = '';
}

```

Production Use Case:

<img width="566" height="337" alt="image" src="https://github.com/user-attachments/assets/2c8b506e-e5e8-418f-9991-4d5a0b55649f" />

-   Live form validation
-   Real-time preview of user data
-   Dynamic UI updates

------------------------------------------------------------------------

### 2️⃣ Settings Toggle (Feature Enable/Disable)

#### Component

``` ts
import {
  AfterViewChecked,
  AfterViewInit,
  Component,
  OnDestroy,
  OnInit,
  ViewChild,
} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 class="card-title mb-3 text-primary">Angular Learning</h1>

      <label
        >Eable Dark Mode
        <input type="checkbox" [(ngModel)]="darkMode" />
      </label>

      <p>Dark Mode Status {{ darkMode }}</p>
    </app-card>
  `,
})
export class AppComponent {
  darkMode: boolean = true;
}

```

<img width="489" height="286" alt="image" src="https://github.com/user-attachments/assets/52f72c3f-6b3a-46a8-b778-186ace3dea8b" />


Production Use Case:

-   Application theme toggle
-   Feature flags
-   User preference settings

------------------------------------------------------------------------

## Important Notes

-   Requires importing `FormsModule`
-   Keeps data synchronized automatically
-   Useful for forms and user input scenarios
-   Overuse in complex apps can impact performance
-   Prefer reactive forms for large-scale enterprise applications

