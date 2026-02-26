# Template Reference Variable

## Definition

A Template Reference Variable in Angular is a way to reference a DOM
element, component instance, or directive directly inside the template.

It is declared using the `#` symbol.

Example:

``` html
<input #username />
```

It allows direct access to template elements without needing ViewChild
in the component class.

------------------------------------------------------------------------

## Analogy

Think of labeling objects in a room.

-   You put a sticky note with a name on a box.
-   Whenever you need that box, you refer to it by that label.

Template reference variables work the same way --- they give a local
name to elements inside the template.

------------------------------------------------------------------------

## Two Production-Level Examples

### 1️⃣ Accessing Input Value Without ngModel

#### Template

``` html
<input #emailInput type="email" placeholder="Enter Email" />
<button (click)="submit(emailInput.value)">Submit</button>
```

#### Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html'
})
export class LoginComponent {

  submit(email: string) {
    console.log("Submitted Email:", email);
  }

}
```

Production Use Case:

-   Lightweight forms
-   Quick input handling
-   Avoiding unnecessary two-way binding

------------------------------------------------------------------------

### 2️⃣ Accessing Component Instance

#### Child Component

``` ts
@Component({
  selector: 'app-counter',
  template: `<p>Count: {{ count }}</p>`
})
export class CounterComponent {
  count = 0;

  increment() {
    this.count++;
  }
}
```

#### Parent Template

``` html
<app-counter #counterComp></app-counter>

<button (click)="counterComp.increment()">
  Increase Count
</button>
```

Production Use Case:

-   Triggering child component methods
-   Managing reusable widgets
-   Dashboard control panels

------------------------------------------------------------------------

## Key Notes

-   Declared using `#variableName`
-   Accessible only inside the template scope
-   Can reference DOM elements, components, or directives
-   Useful for simple interactions
-   For complex logic, prefer `@ViewChild` in component class
