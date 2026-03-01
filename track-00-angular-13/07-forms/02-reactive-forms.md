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

## 2. FormArray Example

A common use case for FormArray is a list of "Skills" or "Phone Numbers" where the user can add or remove fields dynamically.

### Step 1: The TypeScript (register.component.ts)

```ts
export class RegisterComponent implements OnInit {
  registerform!: FormGroup;
  constructor() {}

  ngOnInit(): void {
    this.registerform = new FormGroup({
      userName: new FormControl('', Validators.required),
      email: new FormControl('', [Validators.required, Validators.email]),
      skills: new FormArray([]),
    });
  }

  //Get Control
  get skills(): FormArray {
    return this.registerform.get('skills') as FormArray;
  }

  //Add Control
  AddSkills() {
    this.skills.push(new FormControl('', Validators.required));
  }

  //Review Control
  RemoveSkill(index: number) {
    this.skills.removeAt(index);
  }

  SubmitRegisterForm() {
    if (this.registerform.valid) {
      alert('Registered Successfully!');
    } else {
      alert('Please fill all required fields correctly.');
    }
  }
}

```

### Step 2: The Template (register.component.html)

```html
<form [formGroup]="registerform" (ngSubmit)="SubmitRegisterForm()">

    <div class="mb-3">
        <label for="userName" class="form-label">User Name :</label>
        <input name="userName" id="userName" formControlName="userName" class="form-control">

        <div *ngIf="registerform.get('userName')?.invalid && registerform.get('userName')?.touched">
            <small class="text-danger">Provide Valid User Name</small>
        </div>
    </div>

    <div class="mb-3">
        <label for="email" class="form-label">Email :</label>
        <input name="email" id="email" formControlName="email" class="form-control">

        <div *ngIf="registerform.get('email')?.invalid && registerform.get('email')?.touched">
            <small class="text-danger">Provide Valid Email</small>
        </div>
    </div>

    <div formArrayControl="skills" class="mb-3">
        <label class="form-label">Skills :</label>

        <button class="btn btn-primary" (click)="AddSkills()">+ Add
            Skills</button>

        <div *ngFor="let skill of skills.controls; index as i">
            <input class="form-control" [formControlName]="i" placeholder="Enter Skills">
            <button class="btn btn-warning" (click)="RemoveSkill(i)">Remove
                Skill</button>
        </div>

    </div>
    <br>

    <button class="btn btn-primary" type="submit">Submit Form</button>

</form>
```

### Output

<img width="686" height="873" alt="image" src="https://github.com/user-attachments/assets/6e51b271-baed-4025-b599-98f2289bdb08" />

---

### Validators Class

The Validators class is a collection of built-in functions used to perform synchronous validation on FormControl instances. When a control value changes, these functions run and return either null (valid) or an error object (invalid).

| Validator       | Purpose                                   | Example                                |
|-----------------|-------------------------------------------|----------------------------------------|
| required        | Ensures the field is not empty.           | Validators.required                     |
| email           | Validates that the input matches an email pattern. | Validators.email                        |
| minLength(x)    | Requires a minimum number of characters.  | Validators.minLength(5)                 |
| pattern(regex)  | Matches the input against a Regular Expression. | Validators.pattern('^[0-9]*$')          |


### Key Concepts

- Static Methods: You don't need to create an "instance" of the Validators class; you call its methods directly (e.g., Validators.required).

- Multiple Validators: To apply more than one, wrap them in an array: ['', [Validators.required, Validators.minLength(3)]].

- Validation State: Validators update the status property of the control to either 'VALID', 'INVALID', 'PENDING', or 'DISABLED'.

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
