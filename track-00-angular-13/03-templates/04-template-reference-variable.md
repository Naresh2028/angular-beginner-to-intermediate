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

      <input #emailVariable type="email" placeholder="Enter Email">
      <button type="submit" class="btn btn-primary" (click)="Submit(emailVariable.value)">Submit</button>

    </app-card>
  `,
})
export class AppComponent {

  Submit(email:string){
    console.log("Submitted Email :" + email);
  }
  
}

```

Production Use Case:

<img width="1210" height="551" alt="image" src="https://github.com/user-attachments/assets/ac3fc2f4-aa34-4833-af96-690159956fae" />


-   Lightweight forms
-   Quick input handling
-   Avoiding unnecessary two-way binding

------------------------------------------------------------------------

### 2️⃣ Accessing Component Instance

#### Child Component

``` ts
import { Component, EventEmitter, Input, OnInit, Output } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <p>Count {{count}}</p>
  `
})
export class ChildComponent {

  count = 0;

  increament(){
    this.count++;
  }

}

```

```ts

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
    
    <app-child #childDom></app-child>

    <button (click)="childDom.increament()">
      Increase Count
    </button>

    </app-card>
  `,
})
export class AppComponent {
}


```

<img width="503" height="270" alt="image" src="https://github.com/user-attachments/assets/4ec8a0eb-70fd-4b01-9d6d-033b73ec9ff8" />

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

