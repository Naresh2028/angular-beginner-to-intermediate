# Hierarchical Dependency Injection

## Definition

Hierarchical Dependency Injection means Angular creates a tree of
injectors.

Each level (Root, Module, Component) can have its own injector.

Scopes:

-   Root Level → Single instance shared across entire app
-   Module Level → Shared within a feature module
-   Component Level → New instance per component

This allows fine-grained control over service scope and lifecycle.

------------------------------------------------------------------------

## Analogy

Think of a company structure:

-   Head Office → Global policies (Root Injector)
-   Department → Department-specific tools (Module Injector)
-   Individual Team → Team-specific resources (Component Injector)

Each level can provide its own version of a service.

------------------------------------------------------------------------

## Production-Level Example

### 1️⃣ Root Level Service (Singleton)

By default, when you generate a service in Angular 13, it is configured as a singleton.

- One Instance: Angular creates only one instance of the service class for the entire app.
  
- Shared State: Because every component receives the same instance, they all share the same data. If Component A changes a variable in the service, Component B sees that change immediately.
  
- Memory Efficiency: Since there is only one object in memory, it prevents the overhead of creating duplicate objects for the same logic.

``` ts
@Injectable({
  providedIn: 'root'
})
export class AuthService {}
```

-   One instance across the entire application.

------------------------------------------------------------------------

### 2️⃣ Component Level Service

While services are singletons by default, Angular allows you to break this pattern using Hierarchical Dependency Injection.

- Component-Level Providers: If you list a service in a component's providers: [] array, Angular creates a new, private instance just for that component and its children.

- Multiple Instances: If you have five instances of a component on a page and each provides the service locally, you will have five separate instances of that service instead of one shared singleton.

## Creating Service (Service Provider)
``` ts
import { Injectable } from '@angular/core';

@Injectable()
export class CounterService {
  counter: number = 0;

  private static globalId = 0;

  public instanceId = 0;

  constructor() {
    CounterService.globalId++;
    this.instanceId = CounterService.globalId;
    console.log(`New Instance Created ${this.instanceId} Created!`);
  }

  add() {
    this.counter++;
  }
}

```
## Component (Uitilizing Service) 

``` ts
import { Component, OnInit } from '@angular/core';
import { CounterService } from 'src/app/Services/Counter/counter.service';

@Component({
  selector: 'app-counter',
  template: `
    <div>
      <p>Count : {{ counterService.counter }}</p>
      <p>Component Instance : {{ counterService.instanceId }}</p>
      <button class="btn btn-primary"(click)="counterService.add()">Cilck Me</button>
    </div>
  `,
  styles: [``],
  providers: [CounterService],
})
export class CounterComponent implements OnInit {
  constructor(public counterService: CounterService) {}

  ngOnInit(): void {}
}
```

## App Component 

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>
    <app-card>
      <h1 appHighlight class="card-title mb-3 text-primary">
        Angular Learning
      </h1>
      <h1>Component-Level Provider Test</h1>

      <app-counter></app-counter>
      <app-counter></app-counter>
      <app-counter></app-counter>
    </app-card>
  `,
})
export class AppComponent {}

````

<img width="539" height="714" alt="image" src="https://github.com/user-attachments/assets/af96f8cf-6a71-4c85-ab08-5e2ccc309d7d" />

## What Happened here:
1. Proof of "Multiple Instances"

- In the image, we see Component Instance: 1, 2, and 3.

- Because we added CounterService to the component's providers array, Angular ran the service constructor three separate times.

- Each component is "holding" a completely different object in memory.

2. Proof of "Isolated State"

Look at the Count values in your screenshot:

- Instance 1 has a count of 7.

- Instance 2 has a count of 4.

- Instance 3 has a count of 4.

If this were a Global Singleton (using providedIn: 'root'), all three numbers would be identical (all would be 15, the total sum of clicks). Instead, because they are Component-Level Providers, clicking the button in Instance 1 only affects Instance 1.

Production Use Case:

-   Independent form states
-   Isolated widget configurations
-   Multi-instance dashboards

------------------------------------------------------------------------
