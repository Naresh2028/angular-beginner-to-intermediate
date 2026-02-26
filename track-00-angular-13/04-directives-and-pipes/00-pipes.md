# Pipes

## Definition

Pipes in Angular are used to transform data directly inside the template
without modifying the original value.

They are applied using the pipe (`|`) symbol.

Syntax:

``` html
{{ value | pipeName }}
```

Pipes help format data for display purposes only.

------------------------------------------------------------------------

## Analogy

Think of a water filter.

-   The water remains the same.
-   The filter changes how it appears or is purified.

Similarly, pipes transform how data is displayed without changing the
original data.

------------------------------------------------------------------------

## Two Production-Level Examples

### 1️⃣ Formatting Currency (E-commerce Application)

#### Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-product',
  templateUrl: './product.component.html'
})
export class ProductComponent {

  price: number = 49999;

}
```

#### Template

``` html
<p>Price: {{ price | currency:'INR' }}</p>
```

Production Use Case:

-   Displaying product prices
-   Supporting internationalization
-   Ensuring consistent currency formatting

------------------------------------------------------------------------

### 2️⃣ Date Formatting (Dashboard / Reports)

#### Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-report',
  templateUrl: './report.component.html'
})
export class ReportComponent {

  today: Date = new Date();

}
```

#### Template

``` html
<p>Report Generated On: {{ today | date:'fullDate' }}</p>
```

Production Use Case:

-   Displaying timestamps
-   Showing formatted reports
-   Converting raw date values into readable format

------------------------------------------------------------------------

## Common Built-in Pipes

-   `date`
-   `currency`
-   `percent`
-   `uppercase`
-   `lowercase`
-   `json`
-   `slice`
-   `async`

------------------------------------------------------------------------

## Key Notes

-   Pipes are for presentation logic only.
-   They do not modify the original data.
-   Custom pipes can be created for reusable transformations.
-   Use `async` pipe for Observables to avoid manual subscriptions.
-   Improves template readability and maintainability.


# Async Pipe

## Definition

The `async` pipe in Angular automatically subscribes to an Observable or
Promise and returns the latest emitted value.

It also automatically unsubscribes when the component is destroyed,
preventing memory leaks.

Syntax:

``` html
{{ observable$ | async }}
```

------------------------------------------------------------------------

## Analogy

Think of a live news channel.

-   The reporter (Observable) keeps sending updates.
-   The TV (async pipe) keeps showing the latest news automatically.
-   When you turn off the TV, it stops receiving updates.

The async pipe handles subscription and cleanup automatically.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Real-time User Data (Dashboard)

#### Component

``` ts
import { Component } from '@angular/core';
import { Observable, interval, map } from 'rxjs';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html'
})
export class DashboardComponent {

  liveCount$: Observable<number> = interval(1000).pipe(
    map(value => value + 1)
  );

}
```

#### Template

``` html
<p>Live Counter: {{ liveCount$ | async }}</p>
```

Production Benefits:

-   No manual subscription
-   No need for ngOnDestroy cleanup
-   Cleaner and safer code
-   Ideal for HTTP calls and real-time data streams

------------------------------------------------------------------------

# Custom Pipe

## Definition

A Custom Pipe is a user-defined pipe that transforms data according to
specific business logic.

It implements the `PipeTransform` interface.

Custom pipes are useful when built-in pipes are not sufficient.

------------------------------------------------------------------------

## Analogy

Think of a custom photo filter app.

-   Built-in filters are available.
-   But sometimes you create your own filter for specific needs.

Custom pipes allow you to define your own data transformation logic.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Masking Sensitive Data (User ID)

#### Step 1: Create Pipe

``` ts
@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
    
    <p>Credit Card Number : {{'2314-3215-6541-6545' | maskId}}</p>

    </app-card>
  `,
})
export class AppComponent {
  
}
```

------------------------------------------------------------------------

#### Output

<img width="552" height="226" alt="image" src="https://github.com/user-attachments/assets/c1ef9630-7213-43e8-9796-bef5cb1a803b" />



Production Use Cases:

-   Masking sensitive information
-   Formatting business-specific data
-   Custom UI display transformations
-   Centralizing formatting logic

------------------------------------------------------------------------

## Key Notes

-   `async` pipe manages subscription automatically.
-   Custom pipes implement `PipeTransform`.
-   Pipes should contain only presentation logic.
-   Avoid heavy computation inside pipes.
-   Pure pipes run only when input changes (default behavior).

# Pure Pipe

## Definition

A Pure Pipe in Angular is a pipe that executes only when the input value
changes by reference.

By default, all Angular pipes are pure (`pure: true`).

Pure pipes do NOT run on every change detection cycle --- they run only
when:

-   A primitive value changes
-   An object reference changes

------------------------------------------------------------------------

## Analogy

Think of a security checkpoint that checks only when a new person
enters.

If the same person stands still, the guard does not re-check them.

Pure pipes behave the same way --- they execute only when the input
reference changes.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Sorting Product List

#### Pipe

``` ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'sortProducts',
  pure: true
})
export class SortProductsPipe implements PipeTransform {

  transform(products: any[]): any[] {
    return [...products].sort((a, b) => a.price - b.price);
  }

}
```

------------------------------------------------------------------------

#### Usage

``` html
<li *ngFor="let product of products | sortProducts">
  {{ product.name }} - {{ product.price }}
</li>
```

Production Benefit:

-   Efficient performance
-   Runs only when products array reference changes
-   Ideal for filtering, sorting, formatting

------------------------------------------------------------------------

# Impure Pipe

## Definition

An Impure Pipe runs on every change detection cycle, regardless of
whether the input reference changes.

It is defined using:

``` ts
@Pipe({
  name: 'example',
  pure: false
})
```

Impure pipes are powerful but can impact performance if misused.

------------------------------------------------------------------------

## Analogy

Think of a security guard who re-checks everyone every second, even if
nothing changes.

That constant checking consumes more energy.

Impure pipes behave similarly --- they run frequently.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Real-time Search Filter

#### Pipe

``` ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'filterUsers',
  pure:false
})
export class FilterUsersPipe implements PipeTransform {

    transform(users: any[], searchText: string): any[] {
    if (!searchText) return users;

    return users.filter(user =>
      user.toLowerCase().includes(searchText.toLowerCase())
    );
  }
}

```

------------------------------------------------------------------------

#### Usage

``` ts
import {
  AfterViewChecked, AfterViewInit, Component, OnDestroy, OnInit, ViewChild} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <input [(ngModel)]="searchText" placeholder="Search Users" />

      <li *ngFor="let user of Users | filterUsers:searchText">
        {{ user }}
      </li>
    </app-card>
  `,
})
export class AppComponent {
  Users = ['Naresh', 'Kavin', 'Ben 10'];

  searchText = '';
}

```

<img width="670" height="270" alt="image" src="https://github.com/user-attachments/assets/c3a680c4-7f56-4d1e-a97f-69b5ab656ea0" />

Production Use Case:

-   Real-time search filtering
-   Auto-updating lists
-   Dynamic UI responses

------------------------------------------------------------------------

## Key Differences

| Feature           | Pure Pipe                 | Impure Pipe              |
|-------------------|---------------------------|--------------------------|
| Runs When         | Input reference changes   | Every change detection   |
| Performance       | Optimized                 | Can be expensive         |
| Default Behavior  | Yes                       | No                       |
| Use Case          | Sorting, formatting       | Real-time filtering      |


------------------------------------------------------------------------




## Important Notes

-   Prefer pure pipes for better performance.
-   Avoid heavy computations inside impure pipes.
-   Impure pipes should be used only when necessary.
-   For complex logic, consider moving logic to component or service.
