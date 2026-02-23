# Component Interaction

## Definition

Component interaction refers to how Angular components communicate and exchange data with each other.

In Angular, components are isolated by default. To share data, we must explicitly define communication mechanisms.

Common interaction patterns:

- Parent → Child
- Child → Parent
- Sibling → Sibling (via shared service)

---

## Analogy

Think of a company structure.

- Manager gives tasks to employees.
- Employees report back to the manager.
- Departments communicate through management.

Angular components communicate in a similar structured way.

---

## What Problem It Solves

It solves problems such as:

- Sharing data between UI parts
- Updating parent when child triggers an event
- Passing configuration into reusable components
- Maintaining predictable data flow

Without structured interaction, applications become tightly coupled and hard to maintain.

---

## Minimal Working Example

Parent passes data to child using `@Input()` and listens using `@Output()`.

---

## What Happens If Misconfigured

If component communication is poorly designed:

- Data becomes inconsistent
- Components become tightly coupled
- Hard-to-debug state issues occur
- Application architecture becomes messy

Always follow one-way data flow principles.

---

# Parent → Child

## Definition

Parent to Child communication happens using the `@Input()` decorator.

The parent binds data to a property in the child component.

Data flows downward.

---

## Analogy

Think of a teacher assigning homework.

The teacher (parent) gives instructions.
The student (child) receives and works on them.

---

## What Problem It Solves

It solves the need to:

- Pass configuration values
- Send API response data
- Control child behavior from parent

---

## Minimal Working Example

### Parent Component

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child [message]="parentMessage"></app-child>
  `
})
export class ParentComponent {
  parentMessage = "Hello from Parent";
}
```

### Child Component

```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>{{ message }}</p>`
})
export class ChildComponent {

  @Input() message!: string;

}
```

The child displays data received from the parent.

---

## What Happens If Misconfigured

If:

- `@Input()` is missing
- Property names do not match
- Data types mismatch

You may experience:

- Undefined values
- Template errors
- Unexpected UI behavior

<img width="1622" height="340" alt="image" src="https://github.com/user-attachments/assets/4baafbc9-c4a6-4486-ae84-244885e9ac0c" />


Always ensure proper property binding and type consistency.


