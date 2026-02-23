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

<img width="927" height="432" alt="image" src="https://github.com/user-attachments/assets/0e22ee8e-c3bc-476d-9d33-b9b63446d2cd" />


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

# Child → Parent

## Definition

Child to Parent communication in Angular happens using the `@Output()` decorator along with `EventEmitter`.

The child component emits an event, and the parent listens to that event using event binding.

Data flows upward.

---

## Analogy

Think of a student asking a question in class.

- The student (child) raises a hand.
- The teacher (parent) listens and responds.

The child notifies, the parent reacts.

---

## What Problem It Solves

It solves problems like:

- Notifying parent when a button is clicked
- Sending form data to parent
- Informing parent about state changes
- Triggering parent logic from child actions

---

## Minimal Working Example

### Child Component

```ts
import { Component, EventEmitter, Input, OnInit, Output } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <button (click)="sendEvent()">Click to Send Data</button>
  `
})
export class ChildComponent {

  @Output() notify = new EventEmitter<string>();

  sendEvent(){
    this.notify.emit('Hello Parent');
  }
}

```

### Parent Component

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
  <app-child (notify)="ReceiveData($event)"></app-child>
  `,
  styleUrls: ['./parent.component.css']
})
export class ParentComponent {
  ReceiveData(event:string){
    console.log(event)
  }
}

```

---

## What Happens If Misconfigured

If:

- `@Output()` is missing
- EventEmitter is not emitted
- Event binding name mismatches

You may experience:

<img width="1373" height="852" alt="image" src="https://github.com/user-attachments/assets/509cebf7-194c-4264-bb3d-c64f73b72539" />

- Parent not receiving data
- Silent failures
- Debugging difficulties

Always ensure proper event naming and binding.

---

# Sibling → Sibling (via shared service)

## Definition

Sibling components communicate through a shared service using Observables.

The service acts as a mediator between components.

---

## Analogy

Think of two employees in different rooms.

They don’t talk directly.
They communicate through a common manager (service).

---

## What Problem It Solves

It solves problems like:

- Sharing state between unrelated components
- Global notifications
- Cross-component communication
- Avoiding deeply nested input/output chains

---

## Minimal Working Example

### Shared Service

```ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class CommunicationService {

  private messageSource = new Subject<string>();
  message$ = this.messageSource.asObservable();

  sendMessage(message: string) {
    this.messageSource.next(message);
  }
}
```

---

### Sibling A (Sender)

```ts
import { Component } from '@angular/core';
import { CommunicationService } from './communication.service';

@Component({
  selector: 'app-sibling-a',
  template: `<button (click)="send()">Send</button>`
})
export class SiblingAComponent {

  constructor(private service: CommunicationService) {}

  send() {
    this.service.sendMessage("Hello from Sibling A");
  }
}
```

---

### Sibling B (Receiver)

```ts
import { Component, OnInit, OnDestroy } from '@angular/core';
import { CommunicationService } from './communication.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-sibling-b',
  template: `<p>{{ message }}</p>`
})
export class SiblingBComponent implements OnInit, OnDestroy {

  message = '';
  private subscription!: Subscription;

  constructor(private service: CommunicationService) {}

  ngOnInit() {
    this.subscription = this.service.message$.subscribe(msg => {
      this.message = msg;
    });
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
```

---

## What Happens If Misconfigured

If:

- Service is not provided properly
- Subscriptions are not cleaned up
- Observable logic is incorrect

You may experience:

- Memory leaks
- Stale data
- Unexpected UI behavior

Always unsubscribe in `ngOnDestroy()` and keep services stateless when possible.






