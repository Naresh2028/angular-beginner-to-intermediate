# Template Driven Forms

## Definition

Template Driven Forms in Angular are form implementations where most of
the form logic and validation is handled directly in the template using
directives like:

-   ngModel
-   ngForm
-   ngModelGroup

They are simple, declarative, and suitable for basic to moderate form
requirements.

Template Driven Forms require importing FormsModule.

------------------------------------------------------------------------

## Analogy

Think of filling out a physical paper form.

-   The form structure and validation rules are printed on the paper.
-   You fill it directly based on visible instructions.

Template Driven Forms work similarly --- most logic is defined directly
in the template.

------------------------------------------------------------------------

# Two Production-Level Examples

------------------------------------------------------------------------

## 1️⃣ User Registration Form

### Scenario: Simple user signup with validation

### Component

``` ts
export class RegisterComponent implements OnInit {

  constructor() { }

  regsiterForm = {
    userName : '',
    email:'',
    password:''
  }

  ngOnInit(): void {
  }

  SubmitRegisterForm(form:any){
    if(form.valid){
      alert(`Form submitted successfuly : ${JSON.stringify(form.value)}`);
    }
    alert("Please fill the form")
  }

}
```

------------------------------------------------------------------------

### Template

``` html

<form #formRegister (ngSubmit)="SubmitRegisterForm(formRegister)">

    <div class="form-group mb-3">
        <label for="userName" class="form-label">UserName : </label>
        <input class="form-control" type="text" name="userName"
            id="userName"
            #userName="ngModel"
            [(ngModel)]="regsiterForm.userName" required />
    </div>
    <div class="text-danger" *ngIf="userName.invalid && userName.touched">
        <span >Provide userName</span>
    </div>

    <div class="form-group mb-3">
        <label for="email" class="form-label">Email : </label>
        <input class="form-control" type="email" name="email" id="email"
            #email="ngModel"
            [(ngModel)]="regsiterForm.email" required />
    </div>
    <div class="text-danger" *ngIf="email.invalid && email.touched">
        <span>Provide Email</span>
    </div>

    <div class="form-group mb-3">
        <label for="password" class="form-label">Password : </label>
        <input class="form-control" type="password" name="password"
            id="password"
            #password="ngModel"
            [(ngModel)]="regsiterForm.password"
            required />
    </div>
    <div class="text-danger" *ngIf="password.invalid && password.touched">
        <span>Provide Password</span>
    </div>

    <button class="btn btn-primary" type="submit">Submit Form</button>

</form>
```

### Output

<img width="607" height="837" alt="image" src="https://github.com/user-attachments/assets/ba6805ca-064d-4eca-823d-9b38b6d4f81a" />


------------------------------------------------------------------------

Production Use Case:

-   User registration
-   Contact forms
-   Basic data entry forms

------------------------------------------------------------------------

## 2️⃣ Profile Update Form with ngModelGroup

### Scenario: Updating user profile details

### Component

``` ts
export class ProfileComponent {

  profile = {
    personal: {
      firstName: '',
      lastName: ''
    },
    contact: {
      phone: ''
    }
  };

  updateProfile(form: any) {
    if (form.valid) {
      console.log('Profile Updated:', this.profile);
    }
  }

}
```

------------------------------------------------------------------------

### Template

``` html
<form #profileForm="ngForm" (ngSubmit)="updateProfile(profileForm)">

  <div ngModelGroup="personal">

    <input
      type="text"
      name="firstName"
      [(ngModel)]="profile.personal.firstName"
      required
    />

    <input
      type="text"
      name="lastName"
      [(ngModel)]="profile.personal.lastName"
      required
    />

  </div>

  <div ngModelGroup="contact">

    <input
      type="text"
      name="phone"
      [(ngModel)]="profile.contact.phone"
      required
    />

  </div>

  <button type="submit">
    Update Profile
  </button>

</form>
```

------------------------------------------------------------------------

Production Use Case:

-   User profile editing
-   Settings forms
-   Multi-section forms

------------------------------------------------------------------------

## Key Takeaways

-   Requires FormsModule
-   Uses ngModel for two-way binding
-   Validation handled in template
-   Suitable for small to medium complexity forms
-   Not ideal for very large enterprise-scale forms (Reactive Forms
    preferred)
