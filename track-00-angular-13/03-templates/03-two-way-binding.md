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
import { Component } from '@angular/core';

@Component({
  selector: 'app-profile',
  templateUrl: './profile.component.html'
})
export class ProfileComponent {

  userName: string = "";

}
```

#### Template

``` html
<input type="text" [(ngModel)]="userName" placeholder="Enter Name" />

<p>Preview: {{ userName }}</p>
```

Production Use Case:

-   Live form validation
-   Real-time preview of user data
-   Dynamic UI updates

------------------------------------------------------------------------

### 2️⃣ Settings Toggle (Feature Enable/Disable)

#### Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-settings',
  templateUrl: './settings.component.html'
})
export class SettingsComponent {

  darkMode: boolean = false;

}
```

#### Template

``` html
<label>
  <input type="checkbox" [(ngModel)]="darkMode" />
  Enable Dark Mode
</label>

<p>Dark Mode Status: {{ darkMode }}</p>
```

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
