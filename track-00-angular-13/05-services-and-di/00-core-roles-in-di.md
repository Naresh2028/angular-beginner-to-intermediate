# Core Roles in DI System

## Definition

Angular's Dependency Injection (DI) system is responsible for creating
and supplying dependencies to components and services.

Core roles:

-   Dependency Consumer (DC) → The class that needs a dependency.
-   Dependency Provider (DP) → The configuration that tells Angular how
    to create the dependency.
-   Injector → The engine that resolves and provides dependencies.
-   Tree-shaking → Optimization technique that removes unused services
    from the final bundle.

------------------------------------------------------------------------

## Analogy

Think of a restaurant:

-   Customer (Dependency Consumer) → Orders food.
-   Kitchen (Dependency Provider) → Knows how to prepare the food.
-   Waiter (Injector) → Delivers food to the customer.
-   Tree-shaking → The restaurant removes dishes no one orders to save
    cost.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Logging Service in Enterprise Application

### Dependency Provider (Service)

``` ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LoggerService {

  log(message: string) {
    console.log('[LOG]:', message);
  }

}
```

### Dependency Consumer (Component)

``` ts
import { Component } from '@angular/core';
import { LoggerService } from './logger.service';

@Component({
  selector: 'app-dashboard',
  template: `<button (click)="save()">Save</button>`
})
export class DashboardComponent {

  constructor(private logger: LoggerService) {}

  save() {
    this.logger.log('Dashboard saved successfully');
  }

}
```

------------------------------------------------------------------------

# Singleton Pattern in Software

The Singleton pattern is a design pattern that ensures a class has only one instance throughout the application and provides a global point of access to that instance. It’s commonly used for things like configuration managers, logging, or shared resources where multiple instances could cause inconsistency.


### Key Characteristics
- Only one instance of the class exists.

- Provides a global access point.

- Controls instantiation (usually via a private constructor).

## Singleton Class

```ts
export class AppSingleton {
  private static instance: AppSingleton;

  private constructor() {}

  public static getInstance(): AppSingleton {
    if (!AppSingleton.instance) {
      AppSingleton.instance = new AppSingleton();
    }

    return AppSingleton.instance;
  }

  counter = 0;

  increamentCounter() {
    this.counter++;
  }
}

```

## First Component

```ts
@Component({
  selector: 'app-first-component',
  template: `
    <h2>First Component</h2>
    <div>Count {{ counter }}</div>
    <button (click)="increase()">Click</button>
  `,
})
export class FirstComponentComponent implements OnInit {
  counter!: number;
  singletonInjection = AppSingleton.getInstance();

  constructor() {}

  ngOnInit(): void {}

  increase() {
    this.singletonInjection.increamentCounter();
    this.counter = this.singletonInjection.counter;
  }
}
```

## Second Component

```ts
@Component({
  selector: 'app-second-component',
  template: `
  <h2>Second Component</h2>
    <p>Counter {{ counter }}</p>
    <button (click)="Increase()">Click Here</button>
  `,
})
export class SecondComponentComponent {
  counter: number = 0;

  singletonClass = AppSingleton.getInstance();

  Increase() {
    this.singletonClass.increamentCounter();
    this.counter = this.singletonClass.counter;
  }
}
```

## App Component

```ts
@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 appHighlight class="card-title mb-3 text-primary">
        Angular Learning
      </h1>

      <div #firstComponent>
        <app-first-component></app-first-component>
      </div>

      <div #secondComponent>
        <app-second-component></app-second-component>
      </div>
    </app-card>
  `,
})
export class AppComponent {}
```

## Output

<img width="616" height="461" alt="image" src="https://github.com/user-attachments/assets/a58933c1-686e-4f3e-b69a-ece1575aa608" />



## What Happens in the UI

- First Component shows Count 3 and Second Component shows Counter 2.

- When you click either button, the Singleton’s counter increases by 1.

- Each component updates its local display (this.counter = this.singleton.counter).

- That’s why both counters move forward together — they’re synchronized through the Singleton.


## Analogy

Think of the Singleton as a shared notebook:

- Component A writes “+1” in the notebook.

- Component B opens the same notebook and sees the updated number.

- No matter how many people use it, there’s only one notebook.


## Key Takeaways

- One shared instance

- Global state

- Consistent values across components

