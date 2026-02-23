# 02 — Component Lifecycle Hooks (Part 1)

This file covers key initial lifecycle hooks in Angular 13:
1. Lifecycle hook overview  
2. ngOnChanges()  
3. ngOnInit()  
4. ngDoCheck()
5. ngAfterContentInit()
6. ngAfterContentChecked()
7. ngAfterViewInit()
8. ngAfterViewChecked()
9. ngOnDestroy()

---

## 1️⃣ Lifecycle Hook (Overview)

### Definition
A **lifecycle hook** is a method that Angular calls at a specific point in a component’s existence — from creation, through updates, and before destruction — giving developers a place to run custom logic at predictable moments.

Short memory version:
**Lifecycle hooks are callback points in a component’s life where Angular lets you act.**

---

### Analogy
Imagine a plant’s life cycle:

- **Seed planted** → constructor  
- **Sprout appears** → ngOnInit  
- **Leaves change over time** → ngOnChanges  
- **Growth check everyday** → ngDoCheck  

At each growth stage, a gardener performs specific tasks — just like Angular calls hooks.

---

### What Problem It Solves
Without lifecycle hooks, you would have no structured way to:

- Respond to input value changes
- Initialize component logic once inputs are set
- Detect custom changes during update cycles

Angular would render the UI but give you no reliable moments to insert behavior.

Hooks give you predictable control over a component’s behavior through time.

---

## 2️⃣ ngOnChanges()

### Definition
`ngOnChanges()` is the first lifecycle hook called by Angular — it runs whenever input properties change, including during the initial binding.

---

### Analogy
It’s like **noticing when a student’s grade changes** and reacting immediately.

Whenever input data updates, Angular triggers this hook.

---

### What Problem It Solves
When a component receives new input values from its parent, you may need to run logic *right when* those values change.  
`ngOnChanges()` gives you that exact moment.

Without it, you’d need messy checks in other places.

---

### CLI / Hands-On
There is no specific CLI command for this hook — it is added manually inside a component.

---

### Generated File Structure
If your component is:

```
features/game/lifecycle-demo/
  lifecycle-demo.component.ts
  lifecycle-demo.component.html
```

You add `ngOnChanges()` inside the `.ts` file.

---

### Minimal Working Example

```ts
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `<p>{{ message }}</p>`
})
export class LifecycleDemoComponent implements OnChanges {
  @Input() message!: string;

  ngOnChanges(changes:SimpleChanges){
    console.log('ngOnchanges I received data! :',changes['message'].currentValue); 
  }
}
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>

<app-lifecycle-demo [message]="'Hello from Parent Component...'"></app-lifecycle-demo>`,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'angular-13';
}

```

<img width="916" height="583" alt="image" src="https://github.com/user-attachments/assets/717dc88e-4f5c-4fed-a523-edf65cad047f" />

---



### What Happens If Misconfigured
If you forget `@Input()` on the property:

```ts
message!: string;
```

Angular will not call throw an — *errors*

<img width="1713" height="460" alt="image" src="https://github.com/user-attachments/assets/3cd14358-86f1-476b-ae28-79994e9d67cf" />


---

## 3️⃣ ngOnInit()

### Definition
`ngOnInit()` runs once **after the first ngOnChanges()** and after all input bindings are initialized.

---

### Analogy
Think of creating a new house:

1. The constructor is like laying the foundation and building the walls — the basic structure is ready, but the house isn’t yet livable.

2. ngOnInit() is like moving in and setting up the furniture, electricity, and water supply once the house is built. It’s the stage where everything becomes functional and ready for use.

So in Angular:

1. The constructor sets up the bare minimum (like class properties).

2. ngOnInit() runs after Angular has initialized input properties

---

### What Problem It Solves

Readiness for external data

- ngOnInit() is the right place to fetch data from services or APIs because the component is fully initialized and ready to handle responses.

Safe initialization after inputs are set

- The constructor runs before Angular assigns @Input() values.

- ngOnInit() ensures that all input properties are available, so you don’t run into undefined values when setting up logic.

- `ngOnInit()` is the correct place to initialize logic that uses input properties.

---

### CLI / Hands-On
Add it in your component:

```ts
export class LifecycleDemoComponent implements OnInit {
  ngOnInit(): void {
    // initialization logic here
  }
}
```

---

### Generated File Structure
Same component files as before.

You simply implement the `OnInit` interface and its `ngOnInit()` method.

---

### Minimal Working Example

```ts
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `<p>Init Complete : {{message}}</p>`,
  styleUrls: ['./lifecycle-demo.component.css']
})
export class LifecycleDemoComponent implements OnInit {

  @Input() message!:string;

  ngOnInit(): void {
    console.log('message at ngOninit ' + this.message)
  }

}

```

- From App Component

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>

<app-lifecycle-demo [message]="'This is data from parent component'"></app-lifecycle-demo>`,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'angular-13';
}
```

<img width="799" height="570" alt="image" src="https://github.com/user-attachments/assets/b1f82961-9248-4f94-8fc7-ce3a073e20a6" />


---

### What Happens If Misconfigured
If you place initialization logic in the constructor instead of `ngOnInit()`, input values may still be undefined — because Angular sets them *after* construction.

Example mistake:

```ts
constructor() {
    console.log('message at constructor : ', this.message);
  }
```

This may log `undefined`.

---

<img width="676" height="360" alt="image" src="https://github.com/user-attachments/assets/4881f3e2-2df0-44ff-b2fa-e4961d989abf" />


### Expected Output
When the component loads:

```
message at ngOninit This is data from parent component
```

---

## 4️⃣ ngDoCheck()

### Definition
`ngDoCheck()` is called with every change detection cycle — **as often as Angular checks for changes** — and allows custom change detection logic.

---

### Analogy
It’s like a security guard doing **constant patrols** — checking every area, not just when something officially changes.

---

### What Problem It Solves
Angular’s default change detection doesn’t automatically detect every possible mutation (like deep object changes).  
`ngDoCheck()` lets you run manual checks when needed.

---

### CLI / Hands-On
Add it in your component:

```ts
export class LifecycleDemoComponent implements DoCheck {
  ngDoCheck(): void {
    console.log('ngDoCheck called');
  }
}
```

---

### Generated File Structure
Same component files — just implement ability into `.ts`.

---

### Minimal Working Example

```ts
import { Component, DoCheck, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `<input type="text" placeholder="Enter something..."/>
  <button type="submit" (click)="0">Click Me</button>
  <p>{{checkCount}}</p>
  `,
  styleUrls: ['./lifecycle-demo.component.css']
})
export class LifecycleDemoComponent implements DoCheck {

  checkCount = 0; 

  ngDoCheck():void{
    this.checkCount++;
    console.log('ngDoCheck is triggered count :'+ this.checkCount);
    
  }

}

```
<img width="879" height="805" alt="image" src="https://github.com/user-attachments/assets/c9651ca8-69a9-4aa5-9013-de901f83925f" />

---

### What Happens If Misconfigured
If you write:

```ts
ngDocheck() {}
```

Angular will not call it — silent failure.

<img width="857" height="719" alt="image" src="https://github.com/user-attachments/assets/8a637ff2-2e31-4436-83a4-c04ef41cc160" />


And if you use heavy logic inside `ngDoCheck()`, it may hurt performance — because it's called so frequently.

---

------------------------------------------------------------------------

# ngAfterContentInit()

## Definition

`ngAfterContentInit()` is a lifecycle hook that runs **once** after Angular projects external content into a component using `<ng-content>`.

It is triggered after `@ContentChild` and `@ContentChildren` references are fully initialized.

---

## Analogy

Think of receiving a parcel delivery.

- The house (component) already exists.
- The parcel (projected content) arrives later.
- Only after delivery can you open and use it.

Similarly, projected content becomes available only after Angular inserts it.

---

## What Problem It Solves

It solves the problem of:

- Accessing projected content too early
- Working with `@ContentChild` before initialization
- Needing to manipulate external content safely

Without this hook, projected content may be `undefined`.

---

## Minimal Working Example

### Parent Usage

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>
<app-wrapper>
  <p #projectedContent>Projected Paragraph</p>
</app-wrapper>
`,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'angular-13';
}

```

### Wrapper Component

```ts
import { Component, ContentChild, ElementRef, AfterContentInit } from '@angular/core';

@Component({
  selector: 'app-wrapper',
  template: `<ng-content></ng-content>`
})
export class WrapperComponent implements AfterContentInit {

  @ContentChild('projectedContent') content!: ElementRef;

  ngAfterContentInit() {
    console.log(this.content.nativeElement.textContent);
  }
}
```
Output

<img width="788" height="560" alt="image" src="https://github.com/user-attachments/assets/972dfafb-54d3-464b-bc26-ee53dab378aa" />


---

## What Happens If Misconfigured

If you access `@ContentChild` inside `ngOnInit()`:

- The value will be `undefined`
- Runtime errors can occur
- Unexpected behavior in UI

```ts
export class WrapperComponent implements OnInit, AfterContentInit {

  @ContentChild('projectedContent') content!: ElementRef;


  ngOnInit(): void {
    console.log(this.content.nativeElement.textContent); // TypeError: Cannot read properties of undefined 
  }
  ngAfterContentInit(): void {
    console.log(this.content.nativeElement.textContent);
  }

}
```
<img width="813" height="533" alt="image" src="https://github.com/user-attachments/assets/5be4cc04-676f-4d88-b24f-f25655854fc4" />

---

# ngAfterContentChecked()

## Definition

`ngAfterContentChecked()` runs after Angular checks projected content during every change detection cycle.

It runs:

- After `ngAfterContentInit()`
- Every time projected content changes

---

## Analogy

Think of a security guard checking packages.

Every time a new package arrives or is modified,
the guard inspects it again.

Angular re-checks projected content similarly.

---

## What Problem It Solves

It allows you to:

- React when projected content updates
- Validate content state
- Debug content-related issues

However, it should not contain heavy logic.

---

## Minimal Working Example

Parent Component
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>
<app-wrapper>

    <p>Current Count{{count}}</p>
    <button (click)="increment()">Click</button>

</app-wrapper>
`,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  count = 0;

  increment(){
    this.count++;
  }
}

```

Child Component
```ts
import { AfterContentChecked, Component } from '@angular/core';

@Component({
  selector: 'app-wrapper',
  template: '<ng-content></ng-content>',
  styleUrls: ['./wrapper.component.css']
})
export class WrapperComponent implements AfterContentChecked { 
  ngAfterContentChecked() {
    console.log("Change detection occured.."); 
  }

}

```
Output

<img width="929" height="660" alt="image" src="https://github.com/user-attachments/assets/a2785275-5f47-49e1-b87c-80ca795b6f92" />

---

## What Happens If Misconfigured

If you:

- Perform heavy calculations
- Modify bound properties
- Trigger additional state changes

It can cause:

- Performance issues
- Infinite change detection loops
- ExpressionChangedAfterItHasBeenCheckedError

Use it only for lightweight checks or debugging.


------------------------------------------------------------------------

# ngAfterViewInit()

## Definition

`ngAfterViewInit()` is a lifecycle hook that runs **once** after:

- The component view is fully initialized
- All child component views are initialized
- All `@ViewChild` and `@ViewChildren` queries are resolved

It is the first safe lifecycle hook where you can reliably access DOM elements inside the component template.

---

## Analogy

Imagine setting up a movie theater.

- Constructor → Building the theater  
- ngOnInit → Installing seats and screen  
- ngAfterViewInit → The audience is seated and everything is visible  

Only after everything is fully ready can you interact with what is inside the theater.

---

## What Problem It Solves

It solves problems like:

- Accessing `@ViewChild` too early
- Manipulating DOM before it exists
- Initializing third‑party UI libraries that require DOM access

Without this hook, ViewChild references may be `undefined`.

---

## Minimal Working Example

### Parent Component

```ts
import { AfterViewChecked, AfterViewInit, Component, ViewChild } from '@angular/core';
import { ViewComponentComponent } from './components/view-component/view-component.component';

@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>
<app-view-component></app-view-component>
`,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit {
  
  @ViewChild(ViewComponentComponent) childReference!:ViewComponentComponent;
  
  ngAfterViewInit(): void {
    console.log(this.childReference.childStatus);
    console.log(this.childReference.getAlert()); 
  }
  
}

```

### Child Component

```ts
import { AfterViewInit, Component, ElementRef, ViewChild } from '@angular/core';

@Component({
  selector: 'app-view-component',
  template:`
  <input #username type="text">
  <button (click)="getAlert()">Click
  `,
  styleUrls: ['./view-component.component.css']
})
export class ViewComponentComponent {

  childStatus = 'View Component is ready';

  getAlert(){
    return 'Action from View Component';
  }

}

```

#### Output

<img width="959" height="591" alt="image" src="https://github.com/user-attachments/assets/788c877e-3498-475e-9f92-4913e9faabb8" />


---

## What Happens If Misconfigured

If you try accessing ViewChild inside `ngOnInit()`:

```ts
export class AppComponent implements OnInit,AfterViewInit {
  
  @ViewChild(ViewComponentComponent) childReference!:ViewComponentComponent;
  
  ngAfterViewInit(): void {
    console.log(this.childReference.childStatus);
    console.log(this.childReference.getAlert()); 
  }

  ngOnInit(): void {
    console.log(this.childReference.childStatus);
  }
  
}
```

- The reference may be `undefined`
- Runtime errors may occur
- UI behavior becomes unpredictable

<img width="834" height="554" alt="image" src="https://github.com/user-attachments/assets/0073f0f6-2f6a-4ba6-8fb2-f5c69eca8ae3" />


If you modify bound data inside this hook incorrectly,  
you may encounter `ExpressionChangedAfterItHasBeenCheckedError`.


```ts
@Component({
  selector: 'app-root',
  template:`<h1>Hello World</h1>
<router-outlet></router-outlet>
<h1>Status: {{status}}</h1>
`,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit {
  
  status = 'loading...';
  
  ngAfterViewInit(): void {
    this.status = 'ready'
  }

}
```

#### Output
<img width="1705" height="593" alt="image" src="https://github.com/user-attachments/assets/b286b97a-57b0-4930-beb0-b88b0a406ff1" />

---

# ngAfterViewChecked()

## Definition

`ngAfterViewChecked()` runs after Angular checks the component view during every change detection cycle.

It runs:

- After `ngAfterViewInit()`
- Every time Angular detects changes in the component

---

## Analogy

Imagine a showroom inspector.

Every time something changes in the showroom,  
the inspector walks through and checks everything again.

Angular performs a similar repeated inspection of the component view.

---

## What Problem It Solves

It allows you to:

- React after the view has been re-evaluated
- Perform light validation checks
- Debug view update cycles

However, it should not contain heavy logic.

---

## Minimal Working Example

### Parent Component

```ts
import { AfterViewChecked, AfterViewInit, Component, OnInit, ViewChild } from '@angular/core';
import { ViewComponentComponent } from './components/view-component/view-component.component';

@Component({
  selector: 'app-root',
  template: `<h1>Hello World</h1>
<router-outlet></router-outlet>
<app-view-component></app-view-component>
`,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewChecked {

  @ViewChild(ViewComponentComponent) childReference!: ViewComponentComponent;

  ngAfterViewChecked(): void {    
    console.log(this.childReference.getAlert());
  }


}

```

### Child Component

```ts
import { AfterViewInit, Component, ElementRef, ViewChild } from '@angular/core';

@Component({
  selector: 'app-view-component',
  template: `
  <button (click)="getAlert()">Click
  `,
  styleUrls: ['./view-component.component.css']
})
export class ViewComponentComponent {
  getAlert() {
    return 'Action from View Component';
  }

}

```

### output

<img width="1054" height="630" alt="image" src="https://github.com/user-attachments/assets/353d2400-b1e6-4438-8322-02eb588a7c65" />


Each button click triggers change detection, and the hook runs again.

---

## What Happens If Misconfigured

If you:

- Perform heavy calculations
- Modify bound properties
- Trigger additional state changes

It can cause:

- Performance issues
- Infinite change detection loops
- `ExpressionChangedAfterItHasBeenCheckedError`

Use this hook only for lightweight operations or debugging purposes.

------------------------------------------------------------------------

# ngOnDestroy()

## Definition

`ngOnDestroy()` is a lifecycle hook that runs **once** just before:

-   A component is destroyed
-   Angular removes the component from the DOM
-   The component instance is cleaned up

It is mainly used to perform cleanup tasks such as unsubscribing from
Observables, removing event listeners, or clearing timers.

------------------------------------------------------------------------

## Analogy

Think of renting a hotel room.

-   ngOnInit → You enter the room\
-   Component lifecycle → You stay and use the room\
-   ngOnDestroy → You check out and return the keys

Before leaving, you must clean up your belongings.\
Similarly, before Angular destroys a component, you must clean up
resources.

------------------------------------------------------------------------

## Write a Problem & Fix the Problem by Using the Concept

### ❌ Problem: Memory Leak Due to Unsubscribed Observable

``` ts
import { Component, OnInit } from '@angular/core';
import { interval } from 'rxjs';

@Component({
  selector: 'app-demo',
  template: `<p>Check console</p>`
})
export class DemoComponent implements OnInit {

  ngOnInit() {
    interval(1000).subscribe(value => {
      console.log(value);
    });
  }
}
```

### Why is this a problem?

Even after the component is destroyed,\
the subscription continues running in the background.

This causes: - Memory leaks - Unnecessary processing - Performance
issues

------------------------------------------------------------------------

### ✅ Fix: Use ngOnDestroy to Unsubscribe

``` ts
import { Component, OnInit, OnDestroy } from '@angular/core';
import { interval, Subscription } from 'rxjs';

@Component({
  selector: 'app-demo',
  template: `<p>Check console</p>`
})
export class DemoComponent implements OnInit, OnDestroy {

  private subscription!: Subscription;

  ngOnInit() {
    this.subscription = interval(1000).subscribe(value => {
      console.log(value);
    });
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
```

Now when the component is destroyed,\
the subscription is properly cleaned up.

------------------------------------------------------------------------

## Key Takeaways

-   Runs only once before component destruction
-   Used for cleanup logic
-   Always unsubscribe from Observables
-   Clear timers and event listeners
-   Prevent memory leaks and performance issues



