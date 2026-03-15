# Creational Operators (RxJS)

## Definition

Creation operators in RxJS are used to **create new Observables** from
scratch or from existing values, events, promises, or arrays.

They act as **data sources** that start an observable stream.

Common RxJS Creation Operators:

-   of
-   from
-   fromEvent
-   interval
-   timer
-   defer
-   ajax
-   generate
-   EMPTY
-   throwError

These operators are typically the **starting point of reactive
streams**.

------------------------------------------------------------------------

# Analogy

Think of a **water source** in a city water system.

-   A river → supplies water
-   A reservoir → stores water
-   A pump → pushes water

Similarly, creation operators are **sources that produce data streams**.

------------------------------------------------------------------------

# Common Creation Operators and Scenarios

## 1. of()

### Scenario

Emit a set of predefined values.

Useful when returning mock data or simple observable values.

``` ts
import { of } from 'rxjs';

of(1, 2, 3).subscribe(value => console.log(value));
```

Output:

    1
    2
    3

------------------------------------------------------------------------

## 2. from()

### Scenario

Convert an **array, promise, iterable, or observable-like object** into
an observable.

``` ts
import { from } from 'rxjs';

from([10, 20, 30]).subscribe(value => console.log(value));
```

Useful when working with arrays or promises.

------------------------------------------------------------------------

## 3. fromEvent()

### Scenario

Convert DOM events into observable streams.

``` ts
import { fromEvent } from 'rxjs';

const clicks$ = fromEvent(document, 'click');

clicks$.subscribe(() => console.log("Clicked"));
```

Common use cases:

-   Button clicks
-   Keyboard input
-   Mouse movement

------------------------------------------------------------------------

## 4. interval()

### Scenario

Emit values at fixed time intervals.

``` ts
import { interval } from 'rxjs';

interval(1000).subscribe(value => console.log(value));
```

Output every second:

    0
    1
    2
    3
    ...

Used for:

-   polling APIs
-   timers
-   real-time dashboards

------------------------------------------------------------------------

## 5. timer()

### Scenario

Emit values after a delay.

``` ts
import { timer } from 'rxjs';

timer(3000).subscribe(() => console.log("Executed after 3 seconds"));
```

Used for:

-   delayed actions
-   countdown timers

------------------------------------------------------------------------

## 6. defer()

### Scenario

Create an observable **only when subscribed**.

``` ts
import { defer, of } from 'rxjs';

const observable$ = defer(() => of(Math.random()));

observable$.subscribe(console.log);
observable$.subscribe(console.log);
```

Each subscription produces a new value.

Useful for:

-   lazy execution
-   dynamic data generation

------------------------------------------------------------------------

## 7. ajax()

### Scenario

Make HTTP requests using RxJS.

``` ts
import { ajax } from 'rxjs/ajax';

ajax('https://api.example.com/users')
  .subscribe(response => console.log(response));
```

Used in reactive HTTP workflows.

------------------------------------------------------------------------

## 8. EMPTY

### Scenario

Create an observable that **completes immediately** without emitting
values.

``` ts
import { EMPTY } from 'rxjs';

EMPTY.subscribe({
  complete: () => console.log("Completed")
});
```

Useful for conditional streams.

------------------------------------------------------------------------

## 9. throwError()

### Scenario

Create an observable that emits an error.

``` ts
import { throwError } from 'rxjs';

throwError(() => new Error("Something went wrong"))
  .subscribe({
    error: err => console.error(err.message)
  });
```

Used for:

-   testing error handling
-   simulating failures

------------------------------------------------------------------------

# Key Takeaways

-   Creation operators **start observable streams**
-   They convert values, events, arrays, and promises into observables
-   Often used as **entry points** in RxJS pipelines
-   Combine with **pipe operators** for complex data processing

