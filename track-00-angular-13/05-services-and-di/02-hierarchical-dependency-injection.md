# Hierarchical Dependency Injection

## Definition

Hierarchical Dependency Injection means Angular creates a tree of
injectors.

Each level (Root, Module, Component) can have its own injector.

Scopes:

-   Root Level → Single instance shared across entire app
-   Component Level → New instance per component
-   Module Level → Shared within a feature module

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

## What Happened here:

<img width="539" height="714" alt="image" src="https://github.com/user-attachments/assets/af96f8cf-6a71-4c85-ab08-5e2ccc309d7d" />

---

1. Proof of "Multiple Instances"

- In the image, we see Component Instance: 1, 2, and 3.

- Because we added CounterService to the component's providers array, Angular ran the service constructor three separate times.

- Each component is "holding" a completely different object in memory.

2. Proof of "Isolated State"

- Instance 1 has a count of 7.

- Instance 2 has a count of 4.

- Instance 3 has a count of 4.

If this were a Global Singleton (using providedIn: 'root'), all three numbers would be identical (all would be 15, the total sum of clicks). Instead, because they are Component-Level Providers, clicking the button in Instance 1 only affects Instance 1.

Production Use Case:

-   Independent form states
-   Isolated widget configurations
-   Multi-instance dashboards

------------------------------------------------------------------------
### 3 Module Level Service

In Module Level Services : All components within that same module share one single instance, but components outside that module cannot see it. This is perfect for a "Feature" where all sub-pages need to share the same data.

## 1. Feature Service

```ts
import { Injectable } from '@angular/core';

@Injectable()
export class FeatureService {
  public uniqueId = Math.random();
  public data: string[] = [];

  constructor() {
    console.log('Module-levle instance-created!');
  }
}
```

## 2. Feature Module

We register the service here. Now, every component declared in this module will share the same FeatureService instance.

```ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ComponentAComponent } from './component-a/component-a.component';
import { ComponentBComponent } from './component-b/component-b.component';
import { FeatureService } from 'src/app/Services/feature/feature.service';

@NgModule({
  declarations: [ComponentAComponent, ComponentBComponent],
  imports: [CommonModule],
  exports: [ComponentAComponent, ComponentBComponent],
  providers: [FeatureService],
})
export class FeatureModule {}
```

## 3. Sharing Data Between Siblings

Because they belong to the same module, ComponentA and ComponentB will act like they have a singleton, even though it's restricted to their feature.

## Component - A (Writer)

```ts
import { Component, OnInit } from '@angular/core';
import { FeatureService } from 'src/app/Services/feature/feature.service';

@Component({
  selector: 'app-component-a',
  template: `
    <button class="btn btn-primary" (click)="SendData()">Send Message</button>
  `,
})
export class ComponentAComponent {
  constructor(private featureService: FeatureService) {}

  SendData() {
    this.featureService.data.push(
      'Hello this message from Component - A (Feature Module, & Feature module-level service)',
    );
  }
}

```
## Component-B (Reader)

```ts
import { Component, OnInit } from '@angular/core';
import { FeatureService } from 'src/app/Services/feature/feature.service';

@Component({
  selector: 'app-component-b',
  template: `
    <ul>
      <li *ngFor="let item of featureService.data">{{ item }}</li>
    </ul>
  `,
})
export class ComponentBComponent {
  constructor(public featureService: FeatureService) {}
}
```

## App-Component

````ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>
    <app-card>
      <h1 appHighlight class="card-title mb-3 text-primary">
        Angular Learning
      </h1>
      
      <app-component-a></app-component-a>
      <hr>
      <app-component-b></app-component-b>
    </app-card>
  `,
})
export class AppComponent {}

````

## Output


<img width="552" height="567" alt="image" src="https://github.com/user-attachments/assets/3b766e01-98b0-46d2-bd45-80914b0a6191" />


## Summary of Scope
When you click the button, the text was orginal send from component-A of Feature module.

# Angular Service Provider Levels

| Level     | Syntax                                | Scope                              |
|-----------|---------------------------------------|------------------------------------|
| Root      | `providedIn: 'root'`                  | Entire Application                 |
| Module    | `providers: [Service]` in `@NgModule` | All components in that module      |
| Component | `providers: [Service]` in `@Component`| Only that component & its children |
