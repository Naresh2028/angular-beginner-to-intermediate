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
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'maskId'
})
export class MaskIdPipe implements PipeTransform {

  transform(value: string): string {
    return value.slice(0, 2) + "****" + value.slice(-2);
  }

}
```

------------------------------------------------------------------------

#### Step 2: Use in Template

``` html
<p>User ID: {{ 'ABCD12345678XY' | maskId }}</p>
```

Output:

User ID: AB\*\*\*\*XY

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
