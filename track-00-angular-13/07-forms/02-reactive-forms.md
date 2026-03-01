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
export class LoginComponent {
  loginForm!: FormGroup;

  constructor(
    private fb: FormBuilder,
    private route: Router,
  ) {
    this.loginForm = this.fb.group({
      userName: ['', [Validators.required, Validators.minLength(3)]],
      password: ['', [Validators.required]],
    });
  }

  LoginFormSubmit() {
    if (this.loginForm.valid) {
      alert('Login successfull');
      this.route.navigate(['/users']);
    } else {
      alert('Invalid Form Details!.');
    }
  }
}
```

### Alternate way

```ts
@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css'],
})
export class LoginComponent {
  loginForm: FormGroup;
  constructor(private route: Router) {
    this.loginForm = new FormGroup({
      userName: new FormControl('', [Validators.required, Validators.minLength(3)]),
      password: new FormControl('', [Validators.required]),
    });
  }
}
```

### Key differences

- FormBuilder: this.fb.group({ userName: ['', Validators.required] }) → concise.

- FormGroup + FormControl: new FormGroup({ userName: new FormControl('', Validators.required) }) → explicit.

Both are valid. FormBuilder is preferred in production for brevity, but FormGroup + FormControl is great for learning and when you want to see exactly how Angular constructs the form.

------------------------------------------------------------------------

### Step 3: Template

``` html

<form [formGroup]="loginForm" (ngSubmit)="LoginFormSubmit()">

  <div>
    <label for="userName" class="form-label">User Name :</label>
    <input name="userName" id="userName" formControlName="userName" class="form-control">

    <div *ngIf="loginForm.get('userName')?.invalid && loginForm.get('userName')?.touched">
      <small class="text-danger">Provide Valid User Name</small>
    </div>
  </div>


    <div>
    <label for="password" class="form-label">Password :</label>
    <input name="password" id="password" formControlName="password" class="form-control">

    <div *ngIf="loginForm.get('password')?.invalid && loginForm.get('password')?.touched">
      <small class="text-danger">Provide Valid Password</small>
    </div>
  </div>

  <button type="submit" class="btn btn-primary">
    Submit Form
  </button>

</form>
```


### Output

<img width="735" height="720" alt="image" src="https://github.com/user-attachments/assets/d4e9776c-fc7d-47c2-8113-37bfde7e0387" />


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
