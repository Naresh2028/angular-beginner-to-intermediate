# Forms 

## Definition

Forms in Angular are used to capture, validate, and process user input.

They provide mechanisms to:

-   Bind input fields to data models
-   Validate user input
-   Track form state (valid, invalid, touched, dirty)
-   Submit structured data to backend services

Forms are a core part of interactive web applications.

------------------------------------------------------------------------

## Analogy

Think of a bank application form.

-   You fill in details.
-   The bank verifies your entries.
-   If everything is valid, your application is processed.

Angular forms work the same way --- capture, validate, process.

------------------------------------------------------------------------

## Types

Angular provides two main types of forms:

### 1️⃣ Template-Driven Forms

-   Logic defined mainly in template
-   Uses `ngModel`
-   Requires `FormsModule`
-   Simple and declarative
-   Best for small to medium forms

### 2️⃣ Reactive Forms

-   Logic defined in component class
-   Uses `FormGroup`, `FormControl`
-   Requires `ReactiveFormsModule`
-   Scalable and testable
-   Best for large enterprise forms

------------------------------------------------------------------------

## Structure

### Core Building Blocks

-   FormControl → Tracks individual input field
-   FormGroup → Groups multiple controls
-   FormArray → Manages dynamic controls
-   Validators → Define validation rules

------------------------------------------------------------------------

## Basic Template-Driven Structure

``` html
<form #form="ngForm" (ngSubmit)="onSubmit(form)">

  <input
    type="text"
    name="username"
    [(ngModel)]="user.username"
    required
  />

  <button type="submit">Submit</button>

</form>
```

------------------------------------------------------------------------

## Basic Reactive Form Structure

``` ts
import { FormGroup, FormControl } from '@angular/forms';

form = new FormGroup({
  username: new FormControl('')
});
```

``` html
<form [formGroup]="form" (ngSubmit)="onSubmit()">

  <input formControlName="username" />

  <button type="submit">Submit</button>

</form>
```

------------------------------------------------------------------------

## Form State Properties

-   valid / invalid
-   touched / untouched
-   dirty / pristine
-   pending

These help control UI behavior and validation messages.

------------------------------------------------------------------------

## Key Takeaways

-   Forms manage user input efficiently.
-   Angular provides two major approaches.
-   Template-driven → Simpler.
-   Reactive → More scalable and testable.
-   Choose based on project complexity.
-   Form validation is essential for secure applications.
