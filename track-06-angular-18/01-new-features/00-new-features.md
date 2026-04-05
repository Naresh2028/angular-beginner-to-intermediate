# NEW FEATURES

### 1. Experimental Zoneless Change Detection
### 2. Unified Form Events
### 3. Signal-Based Component APIs

## 1.  Experimental Zoneless Change Detection

This allows you to remove zone.js from your project. Angular 18 introduced a new way for the framework to "hear" about changes without monkey-patching every browser event.

How: Use provideExperimentalZonelessChangeDetection() in your app config.

### Example :

````ts
import { ApplicationConfig, provideExperimentalZonelessChangeDetection, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';
import { provideClientHydration } from '@angular/platform-browser';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
     provideRouter(routes),
      provideClientHydration(),
      provideExperimentalZonelessChangeDetection()
    ]
};

````

Result: Better performance and smaller bundles for the application.

---

## 2. Unified Form Events

Angular 18 added a new .events property to all form controls (FormControl, FormGroup).

Why it's cool: Instead of subscribing to valueChanges and statusChanges separately, you get one stream of events like PristineChangeEvent, TouchedChangeEvent, and ValueChangeEvent.


### Example

````ts

import { Component, OnInit } from '@angular/core';
import {
  FormGroup,
  FormControl,
  ControlEvent,
  ReactiveFormsModule,
  TouchedChangeEvent,
  ValueChangeEvent,
  StatusChangeEvent,
  PristineChangeEvent,
} from '@angular/forms';

@Component({
  selector: 'app-dashboard',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="userForm" (ngSubmit)="SubmittedForm()" novalidate>
      <input
        id="userName"
        name="userName"
        placeholder="userName"
        formControlName="userName"
      />
      <button type="submit">Submit</button>
    </form>
  `,
  styleUrl: './dashboard.component.css',
})
export class DashboardComponent implements OnInit {
  userForm = new FormGroup({
    userName: new FormControl<string>(''),
  });

  ngOnInit(): void {
    this.userForm.events.subscribe((event: ControlEvent) => {
      if (event instanceof ValueChangeEvent) {
        console.log(`Value chnaged ${event.value}`);
      } else if (event instanceof StatusChangeEvent) {
        console.log(`Status Changed ${event.status}`);
      } else if (event instanceof PristineChangeEvent) {
        console.log(`Control is Pristine mode : ${event.pristine}`);
      } else if (event instanceof TouchedChangeEvent) {
        console.log(`Control is pure mode : ${event.touched}`);
      }
    });
  }

  SubmittedForm() {
    console.log(this.userForm.get('userName')?.value);
  }
}

````

### Output

<img width="1275" height="925" alt="image" src="https://github.com/user-attachments/assets/954e305c-53e7-4784-930c-e72041292ebb" />


---

## 3. Signal-Based Component APIs (Stable)

The way you handle data in and out of components has been "Signal-ified":

### input()

- A function used in a Child Component to receive data from a Parent.

- It is a one-way bridge from the Parent to the Child.


### Parent Component 

````ts

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, UsercardComponent],
  template:`
  <app-usercard [userName]="title"> </app-usercard>
<router-outlet />

  `
})
export class AppComponent {
  title = 'Angular 18';
}

````


### Child Component

````ts
@Component({
  selector: 'app-usercard',
  standalone: true,
  imports: [],
  template:`
  <p> <b>Application version : </b>{{userName()}}</p>
  `
})
export class UsercardComponent {
  userName = input<string>('guest');
}

````

### Output

<img width="608" height="304" alt="image" src="https://github.com/user-attachments/assets/26657df6-ed8b-4c3c-a403-744f8215583a" />

## Key points

1. Parent holds the "Source of Truth" (the title variable).

2. Angular detects when the Parent's title changes.

3. The Child "reacts" instantly because input() is a Signal. The Child cannot change this value itself; it can only listen to what the Parent sends.

---

## output()

In Angular 18, output() is the reactive successor to the @Output() decorator and EventEmitter.

### Definition

output() is a function used in a Child Component to define an "event producer." It allows the child to send data or notifications back up to its Parent Component. Unlike the old EventEmitter, it is not an RxJS Observable by default, making it lighter and more efficient.

### example

### Child Component

````ts
@Component({
  selector: 'app-action',
  standalone: true,
  imports: [],
  template: `
    <p>Child Compoenent</p>
    <button (click)="sendMessage()">Send Message</button>
  `,
})
export class ActionComponent {
  dataEmitter = output<string>();

  sendMessage() {
    this.dataEmitter.emit('Hello from Child component...');
  }
}

````

### Parent Component

````ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { UsercardComponent } from '../Components/usercard/usercard.component';
import { ActionComponent } from '../Components/action/action.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, ActionComponent],
  template: `
    <app-action (dataEmitter)="recieveMessage($event)">
      <router-outlet />
    </app-action>
  `,
})
export class AppComponent {
  recieveMessage(event: string) {
    console.log(`Data Recieved : ${event}`);
  }
}

````


### Output
<img width="1161" height="566" alt="image" src="https://github.com/user-attachments/assets/0e943597-ac51-4506-8d30-9e8349f7210c" />


### Key Difference from input()

- input(): Data flows Down (Parent → Child).

- output(): Data flows Up (Child → Parent).

---

## model()

### Definition
A model() is a special type of signal that defines both an Input (to receive data) and an Output (to send data back) in a single line of code.

It is "writable," meaning the child component can change the value directly, and the parent component will be notified automatically. It uses the new Signals API under the hood to ensure the UI updates instantly and efficiently.

### The Problem: The "Traditional" Way Drawback

Before model(), if you wanted to create a custom component with two-way binding (like a toggle switch), you had to write a "Pattern" called **De-sugaring**.

### The Drawbacks:

1. Boilerplate: You had to define an @Input() to get the value AND an @Output() to send it back.

2. Manual Updates: You had to manually call this.checkedChange.emit(newValue) every time the user interacted with the UI.

### Child Component

```ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-toggle',
  standalone: true,
  imports: [],
  template: `
    <button (click)="ChangeToggle()">Click</button>
    <P>STAUS : {{ checked }}</P>
  `,
})
export class ToggleComponent {
  @Input() checked = false;
  @Output() checkedChange = new EventEmitter<boolean>();

  ChangeToggle() {
    this.checked = !this.checked;
    this.checkedChange.emit(this.checked);
    console.log('clicked : ' + this.checked);
  }
}

````

### Parent Component

````ts
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ToggleComponent],
  template: `
    <app-toggle [(checked)]="isAdmin"></app-toggle>
    <div>Current Status : {{ isAdmin }}</div>
  `,
})
export class AppComponent {
  isAdmin: boolean = true;
}
````

## 2. The New model() Way (v17.2+)

This is the Signal-based version. It removes the need for EventEmitter and decorators.

### Child Component (app-toggle.component.ts)

```ts
@Component({
  selector: 'app-toggle',
  standalone: true,
  imports: [],
  template: `
    <button (click)="ChangeToggle()">Click</button>
    <P>STAUS : {{ checked() }}</P>
  `,
})
export class ToggleComponent {
  //Replaced @Input(), @Outpu(), & Event Emitter in a sinlge Line code.
  checked = model(false);

  ChangeToggle() {
    this.checked.update((val) => !val);
    console.log(this.checked());
  }
}
```


### Parent-Component 

```ts
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ToggleComponent],
  template: `
    <app-toggle [(checked)]="isAdmin"></app-toggle>
    <div>Current Status : {{ isAdmin() }}</div>
  `,
})
export class AppComponent {
  isAdmin = signal(true);
}
```


### Output

<img width="1105" height="691" alt="image" src="https://github.com/user-attachments/assets/72a0511a-70e5-4d68-b57b-1523fc0b5695" />

###  Summary

- model() is the "Smart" version of an Input(), and Output().

- Auto-Sync: When the child changes the value, the parent's variable updates automatically.

- The Rule: You must use the "Banana-in-a-box" syntax [( )] in the HTML to turn on the two-way sync.

