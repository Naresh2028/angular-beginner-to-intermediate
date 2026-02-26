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
import {
  AfterViewChecked,
  AfterViewInit,
  Component,
  OnDestroy,
  OnInit,
  ViewChild,
} from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 class="card-title mb-3 text-primary">Angular Learning</h1>

      <img
        [src]="imageUrl"
        alt="Angular Image"
        class="img-fluid rounded mb-3"
      />

      <button class="btn btn-primary w-100" [disabled]="isDisabled">
        Subscribe Me
      </button>
    </app-card>
  `,
})
export class AppComponent {
  imageUrl =
    'assets/Images/ToyStory.jfif';
  isDisabled = true;
}

```

What happens:

<img width="633" height="566" alt="image" src="https://github.com/user-attachments/assets/38a601ba-e61c-4699-9341-162e2dc01f57" />


-   The image source updates dynamically
-   The button becomes disabled based on component state

------------------------------------------------------------------------

## Key Notes

-   Uses square bracket syntax `[property]`
-   Updates DOM properties, not attributes
-   One-way data binding (component → view)
-   Automatically updates when component data changes
-   Frequently used with `@Input()` for parent → child communication


# DOM Properties in HTML

## Definition

DOM Properties represent the live state of an HTML element inside the
browser's Document Object Model (DOM).

They are part of the JavaScript object representation of an element and
can change dynamically while the application is running.

Properties reflect the current value/state of an element.

------------------------------------------------------------------------

## Analogy

Think of a smart TV remote.

-   The TV has current volume, brightness, and channel.
-   These values can change anytime.

DOM properties behave like those live values --- they represent the
current state.

------------------------------------------------------------------------

## Scenario to Use

Use DOM properties when:

-   Enabling or disabling a button dynamically
-   Updating input field values
-   Changing image sources at runtime
-   Checking checkbox states
-   Modifying element behavior via JavaScript

------------------------------------------------------------------------

## Practical Example

``` html
<input id="username" value="Naresh" />
<button id="btn">Click</button>
```

``` js
const input = document.getElementById("username");
const button = document.getElementById("btn");

input.value = "Updated Name";   // Updating property
button.disabled = true;         // Changing live state
```

Here, `value` and `disabled` are DOM properties.

------------------------------------------------------------------------

# DOM Attributes in HTML

## Definition

DOM Attributes define the initial configuration of an element in the
HTML markup.

They represent how the element was originally declared in the HTML
document.

Attributes are part of the HTML source code.

------------------------------------------------------------------------

## Analogy

Think of a birth certificate.

-   It records original information.
-   It does not automatically change during life.

DOM attributes represent the original definition.

------------------------------------------------------------------------

## Scenario to Use

Use DOM attributes when:

-   Defining static values in HTML
-   Setting accessibility attributes (`aria-*`)
-   Configuring metadata
-   Defining default element behavior

------------------------------------------------------------------------

## Practical Example

``` html
<input type="text" value="Initial Value" disabled />
```

In this example:

-   `type`, `value`, and `disabled` are HTML attributes.
-   They define the initial state of the element.

------------------------------------------------------------------------

# Attributes vs Properties

| Feature               | DOM Attribute        | DOM Property                  |
|------------------------|----------------------|-------------------------------|
| Defined In             | HTML markup          | JavaScript DOM object         |
| Represents             | Initial value        | Current live state            |
| Updates Automatically  | No                   | Yes                           |
| Example                | `value="Hello"`      | `input.value = "Hello"`       |
| Boolean Example        | `disabled`           | `button.disabled = true`      |


------------------------------------------------------------------------

## Key Difference

-   Attribute → Defines how the element starts.
-   Property → Represents how the element behaves right now.

In dynamic applications (like Angular), property binding is preferred
because it updates the live state of the element.

