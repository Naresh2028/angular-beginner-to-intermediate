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

  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges called:', changes);
  }
}
```

---

### What Happens If Misconfigured
If you forget `@Input()` on the property:

```ts
message!: string;
```

Angular will not call `ngOnChanges()` — *no errors*, just silent failure.

If you use the wrong signature:

```ts
ngOnchange(changes: any) {}
```

Angular will not recognize it — again silent failure.

---

### Expected Output
When the parent changes the value of `message`, the console should show:

```
ngOnChanges called: { message: { previousValue: ..., currentValue: ... } }
```

---

## 3️⃣ ngOnInit()

### Definition
`ngOnInit()` runs once **after the first ngOnChanges()** and after all input bindings are initialized.

---

### Analogy
It’s like **warming up before the game starts** — ready to run logic that depends on inputs being available.

---

### What Problem It Solves
At creation time, Angular separates construction (class instantiation) from initialization (inputs ready).  
You should *never* run initialization logic in the constructor — because input values may not be set yet.

`ngOnInit()` is the correct place to initialize logic that uses input properties.

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
  template: `<p>Init Complete: {{ message }}</p>`
})
export class LifecycleDemoComponent implements OnInit {
  @Input() message = '';

  ngOnInit(): void {
    console.log('ngOnInit: message is', this.message);
  }
}
```

---

### What Happens If Misconfigured
If you place initialization logic in the constructor instead of `ngOnInit()`, input values may still be undefined — because Angular sets them *after* construction.

Example mistake:

```ts
constructor() {
  console.log('message at constructor:', this.message);
}
```

This may log `undefined`.

---

### Expected Output
When the component loads:

```
ngOnInit: message is Hello
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
