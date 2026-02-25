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


# DOM Properties

## Definition

DOM Properties represent the current state of an element in the Document
Object Model (DOM).

They are part of the live DOM object and can change dynamically as the
application runs.

In Angular, property binding updates DOM properties using:

`[property]="expression"`

------------------------------------------------------------------------

## Analogy

Think of a light switch.

-   The switch position (ON/OFF) represents the current state.
-   That state can change anytime.

DOM properties represent the live state of an element.

------------------------------------------------------------------------

## Scenario to Use

Use DOM properties when:

-   Enabling or disabling a button
-   Setting input values dynamically
-   Binding image sources
-   Updating element states dynamically

------------------------------------------------------------------------

## Practical Example

``` html
<button [disabled]="isDisabled">Submit</button>
<input [value]="username" />
<img [src]="imageUrl" />
```

These update the actual live state of the elements.

------------------------------------------------------------------------

# DOM Attributes

## Definition

DOM Attributes define the initial values set in the HTML markup.

They represent how the element was originally defined in the HTML file.

Attributes do not automatically reflect runtime changes unless
explicitly updated.

------------------------------------------------------------------------

## Analogy

Think of a birth certificate.

-   It records initial details.
-   It does not automatically update when life changes.

DOM attributes represent the original setup of an element.

------------------------------------------------------------------------

## Scenario to Use

Use DOM attributes when:

-   Defining static values
-   Setting accessibility attributes (`aria-*`)
-   Working with non-standard attributes
-   Using Angular attribute binding `[attr.attributeName]`

------------------------------------------------------------------------

## Practical Example

``` html
<input type="text" value="Initial Value">
<button disabled>Submit</button>
```

Angular attribute binding example:

``` html
<button [attr.aria-label]="buttonLabel">Click</button>
```

------------------------------------------------------------------------

# Attribute vs Property

  Feature               DOM Property          DOM Attribute
  --------------------- --------------------- --------------------------------
  Represents            Current live state    Initial HTML value
  Updates dynamically   Yes                   No (unless explicitly updated)
  Angular binding       `[property]`          `[attr.attribute]`
  Example               `[disabled]="true"`   `disabled`

------------------------------------------------------------------------

## Key Difference

-   Property = Live, dynamic, reflects runtime state.
-   Attribute = Static, initial configuration.

In Angular, property binding is used most of the time for dynamic
behavior.
