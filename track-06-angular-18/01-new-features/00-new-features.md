
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

model(): Enables easy two-way data binding with signals.

effect(): An effect() is a block of code that registers itself as a consumer of any Signal used inside it.
