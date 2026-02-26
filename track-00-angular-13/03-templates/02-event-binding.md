# Event Binding

## Definition

Event Binding in Angular allows you to listen to DOM events and respond
to them using component methods.

It uses parentheses syntax:

`(event)="method()"`

When the specified event occurs, Angular executes the corresponding
method in the component class.

------------------------------------------------------------------------

## Analogy

Think of a doorbell.

-   When someone presses the bell (event),
-   The sound is triggered (action).

Event binding works the same way --- when an event happens in the UI, a
function runs in the component.

------------------------------------------------------------------------

## Scenario to Use

Use event binding when:

-   Handling button clicks
-   Capturing user input
-   Responding to form submissions
-   Listening to keyboard or mouse events
-   Triggering business logic from UI interaction

------------------------------------------------------------------------

## Practical Example

### Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-demo',
  templateUrl: './demo.component.html'
})
export class DemoComponent {

  count = 0;

  increment() {
    this.count++;
  }

}
```

### Template (demo.component.html)

``` html
<button (click)="increment()">
  Click Me
</button>

<p>Count: {{ count }}</p>
```

What happens:

-   When the button is clicked
-   The `increment()` method executes
-   The count updates automatically in the UI

------------------------------------------------------------------------

## Common Event Examples

``` html
<input (input)="onInputChange($event)" />
<form (submit)="onSubmit()" />
<div (mouseover)="onHover()"></div>
```

------------------------------------------------------------------------

## Key Notes

-   Uses parentheses syntax `(event)`
-   Connects DOM events to component methods
-   Supports passing `$event` object
-   Part of one-way data flow (View → Component)
-   Often combined with property binding for interactive UI
