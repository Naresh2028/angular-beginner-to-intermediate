# 02 — Component Lifecycle Hooks (Part 1)

This file covers key initial lifecycle hooks in Angular 13:
1. Lifecycle hook overview  
2. ngOnChanges()  
3. ngOnInit()  
4. ngDoCheck()

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

```
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
import { Component, DoCheck } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `<p>DoCheck is running</p>`
})
export class LifecycleDemoComponent implements DoCheck {

  ngDoCheck(): void {
    console.log('ngDoCheck executed');
  }
}
```

---

### What Happens If Misconfigured
If you write:

```ts
ngDocheck() {}
```

Angular will not call it — silent failure.

And if you use heavy logic inside `ngDoCheck()`, it may hurt performance — because it's called so frequently.

---

### Expected Output
When change detection runs (even without input changes), the console should log:

```
ngDoCheck executed
```

This confirms Angular runs this hook every detection cycle.

---

## Summary
- `ngOnChanges()` runs whenever input values change.  
- `ngOnInit()` runs once after first binding.  
- `ngDoCheck()` runs every change detection cycle — use with care.

Keep this file as **Part 1 of Lifecycle Hooks**.







