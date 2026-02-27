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

# Service Definition & Metadata

## Definition

A Service in Angular is a class that contains reusable business logic.

Metadata is defined using the `@Injectable()` decorator, which tells
Angular:

-   How the service should be provided
-   Where it should be available
-   How it participates in the DI system

------------------------------------------------------------------------

## Analogy

Think of a shared utility room in an office.

-   Multiple employees use it.
-   It centralizes common tools.
-   It avoids duplication across departments.

Services centralize logic for reuse across components.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: API Data Service

``` ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'https://api.example.com/users';

  constructor(private http: HttpClient) {}

  getUsers(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl);
  }

}
```

### Using Service in Component

``` ts
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-users',
  template: `
    <ul>
      <li *ngFor="let user of users">
        {{ user.name }}
      </li>
    </ul>
  `
})
export class UsersComponent implements OnInit {

  users: any[] = [];

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userService.getUsers().subscribe(data => {
      this.users = data;
    });
  }

}
```

------------------------------------------------------------------------

## Key Takeaways

-   DI promotes loose coupling.
-   Injector resolves dependencies automatically.
-   `providedIn: 'root'` enables tree-shaking.
-   Services centralize reusable logic.
-   Metadata controls service lifecycle and scope.

