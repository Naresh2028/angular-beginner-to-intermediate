# Reactive Forms

## Definition

Reactive Forms in Angular are model-driven forms where form structure,
validation, and business logic are defined in the component class.

They use:

-   FormControl
-   FormGroup
-   FormArray
-   Validators

Reactive Forms require importing `ReactiveFormsModule`.

They provide better scalability, testability, and control compared to
Template-Driven Forms.

------------------------------------------------------------------------

## Analogy

Think of building a machine using a blueprint.

-   The blueprint (component class) defines the entire structure.
-   The UI (template) simply connects to the machine.

Reactive Forms work the same way --- logic lives in the component,
template reflects it.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Enterprise Login Form with Validation

### Step 1: Import Module

``` ts
import { ReactiveFormsModule } from '@angular/forms';
```

------------------------------------------------------------------------

### Step 2: Component Implementation

``` ts
import { Component } from '@angular/core';
import { FormGroup, FormControl, Validators } from '@angular/forms';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html'
})
export class LoginComponent {

  loginForm = new FormGroup({
    email: new FormControl('', [
      Validators.required,
      Validators.email
    ]),
    password: new FormControl('', [
      Validators.required,
      Validators.minLength(6)
    ])
  });

  onSubmit() {
    if (this.loginForm.valid) {
      console.log('Login Data:', this.loginForm.value);
    }
  }

}
```

------------------------------------------------------------------------

### Step 3: Template

``` html
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">

  <input type="email" formControlName="email" />
  <div *ngIf="loginForm.get('email')?.invalid && loginForm.get('email')?.touched">
    Valid email is required
  </div>

  <input type="password" formControlName="password" />
  <div *ngIf="loginForm.get('password')?.invalid && loginForm.get('password')?.touched">
    Password must be at least 6 characters
  </div>

  <button type="submit" [disabled]="loginForm.invalid">
    Login
  </button>

</form>
```

------------------------------------------------------------------------

## Why This Is Production-Grade

-   Strong validation control
-   Explicit form model
-   Easy unit testing
-   Scales for complex enterprise forms
-   Supports dynamic forms using FormArray
-   Clear separation of logic and presentation

------------------------------------------------------------------------

## Key Takeaways

-   Reactive Forms are model-driven.
-   Validation logic resides in the component.
-   Best for large-scale and complex applications.
-   Highly testable and maintainable.
-   Prefer Reactive Forms in enterprise Angular projects.
