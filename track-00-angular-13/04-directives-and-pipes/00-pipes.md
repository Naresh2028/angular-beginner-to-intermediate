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
