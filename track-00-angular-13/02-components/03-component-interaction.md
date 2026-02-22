# Component Interaction

## Definition

Component interaction refers to the way Angular components communicate
and share data with each other.\
This usually happens between:

-   Parent → Child
-   Child → Parent
-   Sibling components (through a shared service)

It is a core concept in building scalable Angular applications.

------------------------------------------------------------------------

## Analogy

Think of a family in a house.

-   Parent gives instructions to child.
-   Child responds back to parent.
-   Siblings talk through parents.

Components behave the same way in Angular.

------------------------------------------------------------------------

## Practical Example & Usage

In real applications:

-   Passing user data to a profile component
-   Sending selected product to a cart component
-   Emitting form submission events
-   Updating parent dashboard from child widget

------------------------------------------------------------------------

## Key Takeaways

-   Angular components are isolated by default
-   Data must be passed explicitly
-   Use `@Input()` for Parent → Child
-   Use `@Output()` for Child → Parent
-   Use services for sibling communication

------------------------------------------------------------------------

# Parent to Child Communication

## Definition

Parent to Child communication happens using the `@Input()` decorator.

The parent sends data to the child through property binding.

------------------------------------------------------------------------

## Analogy

Imagine a teacher giving homework to students.

The teacher (parent) provides instructions. Students (child) receive and
use them.

------------------------------------------------------------------------

## Practical Example & Usage

### Parent Component

``` ts
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

``` ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>{{ message }}</p>`
})
export class ChildComponent {

  @Input() message!: string;

}
```

Now the child displays the data received from the parent.

------------------------------------------------------------------------

## Key Takeaways

-   Use `@Input()` decorator
-   Data flows downward
-   Child should not modify parent data directly
-   Follow one-way data flow principle

------------------------------------------------------------------------

# Child to Parent Communication

## Definition

Child to Parent communication happens using the `@Output()` decorator
and `EventEmitter`.

The child emits an event. The parent listens and reacts.

------------------------------------------------------------------------

## Analogy

Imagine a student asking a question in class.

Student (child) raises hand. Teacher (parent) responds.

------------------------------------------------------------------------

## Practical Example & Usage

### Child Component

``` ts
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <button (click)="sendMessage()">Send to Parent</button>
  `
})
export class ChildComponent {

  @Output() notifyParent = new EventEmitter<string>();

  sendMessage() {
    this.notifyParent.emit("Hello Parent!");
  }
}
```

### Parent Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child (notifyParent)="receiveMessage($event)"></app-child>
  `
})
export class ParentComponent {

  receiveMessage(message: string) {
    console.log(message);
  }
}
```

Now the parent receives data emitted from the child.

------------------------------------------------------------------------

## Key Takeaways

-   Use `@Output()` decorator
-   Use `EventEmitter`
-   Data flows upward
-   Parent handles emitted events
-   Keeps components loosely coupled
