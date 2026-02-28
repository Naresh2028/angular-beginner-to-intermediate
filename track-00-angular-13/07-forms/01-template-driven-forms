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
import { Component } from '@angular/core';

@Component({
  selector: 'app-register',
  templateUrl: './register.component.html'
})
export class RegisterComponent {

  user = {
    name: '',
    email: '',
    password: ''
  };

  onSubmit(form: any) {
    if (form.valid) {
      console.log('Form Submitted:', this.user);
    }
  }

}
```

------------------------------------------------------------------------

### Template

``` html
<form #form="ngForm" (ngSubmit)="onSubmit(form)">

  <input
    type="text"
    name="name"
    [(ngModel)]="user.name"
    required
    #name="ngModel"
  />
  <div *ngIf="name.invalid && name.touched">
    Name is required
  </div>

  <input
    type="email"
    name="email"
    [(ngModel)]="user.email"
    required
    email
    #email="ngModel"
  />
  <div *ngIf="email.invalid && email.touched">
    Valid email required
  </div>

  <input
    type="password"
    name="password"
    [(ngModel)]="user.password"
    required
    minlength="6"
    #password="ngModel"
  />
  <div *ngIf="password.invalid && password.touched">
    Password must be at least 6 characters
  </div>

  <button type="submit" [disabled]="form.invalid">
    Register
  </button>

</form>
```

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
