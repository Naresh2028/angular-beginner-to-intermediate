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

