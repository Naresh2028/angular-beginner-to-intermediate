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

<form #formRegister="ngForm" (ngSubmit)="SubmitRegisterForm(formRegister)">

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
  Profile = {
    person: {
      firstName: '',
      lastName: '',
    },
    contact: {
      number: '',
    },
  };

  SubmitRegisterForm(form: any) {
    if (form.valid) {
      alert('You Form Submitted Successfully');
    } else {
      alert('Provide Valid Form!');
    }
  }
}
```

------------------------------------------------------------------------

### Template

``` html
<form #ProfileForm="ngForm" (ngSubmit)="SubmitRegisterForm(ProfileForm)">

  <div class="mb-3 form-group" ngModelGroup="person">
    <label class="form-label" for="firstName">First Name</label>
    <input type="text" name="firstName" class="form-control"
      [(ngModel)]="Profile.person.firstName" placeholder="Enter First Name"
      #firstName="ngModel" required>

    <div class="text-danger" *ngIf="firstName.invalid && firstName.touched">
      provide firstName
    </div>

    <label class="form-label" for="lastName">Last Name</label>
    <input type="text" name="lastName" class="form-control"
      [(ngModel)]="Profile.person.lastName" placeholder="Enter Last Name"
      #lastName="ngModel" required>

    <div class="text-danger" *ngIf="lastName.invalid && lastName.touched">
      provide lastName
    </div>
  </div>

  <div ngModelGroup="contact"> 
    <label class="form-label" for="number">Phone</label>
    <input type="text" name="number" class="form-control"
      [(ngModel)]="Profile.contact.number" placeholder="Enter number"
      #number="ngModel" required>

    <div class="text-danger" *ngIf="number.invalid && number.touched">
      provide number
    </div>
  </div>


  <button class="btn btn-primary">Submit Form</button>

</form>
```

### Output

<img width="777" height="889" alt="image" src="https://github.com/user-attachments/assets/43a0c6b9-c647-4179-ab23-2dc5ebcef2b5" />


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
