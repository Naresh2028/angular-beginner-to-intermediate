# Content Projection

## Definition

Content Projection allows you to pass external HTML content into a
component using `<ng-content>`.

It enables reusable components where:

-   The internal layout is fixed
-   The inserted content is dynamic
-   Parent components control what gets displayed inside

------------------------------------------------------------------------

## Analogy

Think of a reusable gift box.

-   The box design stays the same.
-   The gift placed inside can change.

The component is the box. The projected content is the gift inside.

------------------------------------------------------------------------

## What Problem It Solves

It solves problems such as:

-   Making reusable UI containers (cards, modals, panels)
-   Allowing flexible layout structures
-   Avoiding rigid hardcoded templates
-   Supporting customizable component content

Without content projection, components become less reusable and more
tightly coupled.

------------------------------------------------------------------------

## Minimal Working Example

### Step 1: Create Reusable Card Component

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <ng-content></ng-content>
    </div>
  `
})
export class CardComponent {}
```

------------------------------------------------------------------------

### Step 2: Use the Component

``` html
<app-card>
  <h2>Card Title</h2>
  <p>This content is projected inside the card.</p>
</app-card>
```

What happens:

-   The `<h2>` and `<p>` elements are inserted into `<ng-content>`
-   The wrapper layout remains reusable

------------------------------------------------------------------------

## What Happens If Misconfigured

If:

-   `<ng-content>` is missing
-   Incorrect selector usage in multi-slot projection
-   Projected content is accessed too early (before content hooks)

You may experience:

-   Content not rendering
-   Layout breaking
-   Unexpected DOM behavior

For projected content logic, use `ngAfterContentInit()` safely.

------------------------------------------------------------------------

## Key Notes

-   Uses `<ng-content>`
-   Makes components highly reusable
-   Supports single-slot and multi-slot projection
-   Keeps layout and content responsibilities separate
