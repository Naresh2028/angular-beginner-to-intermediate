# Property Binding

## Definition

Property Binding in Angular is used to bind a component property to a
DOM element property using square bracket syntax:

`[property]="expression"`

It allows dynamic updates from the component class to the template
element properties.

------------------------------------------------------------------------

## Analogy

Think of a remote controlling a TV.

<img width="666" height="1000" alt="image" src="https://github.com/user-attachments/assets/84599d0c-3803-4014-a5ce-5bab3b225750" />


-   The remote (component) sends signals.
-   The TV (DOM element) updates accordingly.

When the remote changes the channel, the TV reflects it instantly.

------------------------------------------------------------------------

## Scenario to Use

Use property binding when:

-   Dynamically setting element properties
-   Binding image sources
-   Enabling/disabling buttons
-   Setting CSS classes or styles
-   Passing data to child components

It is used when updating DOM properties, not HTML attributes.

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

  imageUrl = "https://via.placeholder.com/150";
  isDisabled = true;

}
```

### Template (demo.component.html)

``` html
<img [src]="imageUrl" />

<button [disabled]="isDisabled">
  Click Me
</button>
```

What happens:

-   The image source updates dynamically
-   The button becomes disabled based on component state

------------------------------------------------------------------------

## Key Notes

-   Uses square bracket syntax `[property]`
-   Updates DOM properties, not attributes
-   One-way data binding (component → view)
-   Automatically updates when component data changes
-   Frequently used with `@Input()` for parent → child communication

