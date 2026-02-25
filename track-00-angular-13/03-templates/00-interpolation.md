# Interpolation

## Definition

Interpolation in Angular is a template syntax used to bind component
data to the HTML view using double curly braces:

`{{ expression }}`

It allows you to display dynamic data from the component class inside
the template.

------------------------------------------------------------------------

## Analogy

Think of a name badge template.

<img width="275" height="183" alt="image" src="https://github.com/user-attachments/assets/6a786c81-9a50-4126-a83c-5ea8cb514649" />


The badge design is fixed, but the name printed on it changes depending
on the person.

The template is the badge. The variable value is the name printed
dynamically.

------------------------------------------------------------------------

## Scenario to Use

Use interpolation when:

-   Displaying text values from the component
-   Showing API response data
-   Rendering dynamic titles, labels, counters
-   Evaluating simple expressions in the template

It is best suited for displaying string or primitive values.

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

  name = "Naresh";
  count = 5;

}
```

### Template (demo.component.html)

``` html
<h2>Hello {{ name }}</h2>
<p>Total Count: {{ count }}</p>
<p>Next Count: {{ count + 1 }}</p>
```

Output:

-   Displays dynamic name
-   Shows variable values
-   Evaluates simple expressions

------------------------------------------------------------------------

## Key Notes

-   Uses `{{ }}` syntax
-   Supports expressions (not statements)
-   Cannot contain complex logic
-   Automatically updates when data changes
-   Follows one-way data binding (component → view)

