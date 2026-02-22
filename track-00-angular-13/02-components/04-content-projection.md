# Content Projection

## Definition

Content projection allows you to pass external HTML content into a
component using `<ng-content>`.

It enables reusable components where the internal layout is fixed, but
the content inside can be dynamic.

------------------------------------------------------------------------

## Analogy

Think of a gift box.

-   The box design is fixed.
-   The items placed inside can change.

The component is the box. The projected content is the gift inside.

------------------------------------------------------------------------

## Practical Example & Usage

### Card Component

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

### Usage

``` html
<app-card>
  <h2>Title</h2>
  <p>This content is projected inside the card.</p>
</app-card>
```

The `<h2>` and `<p>` elements are inserted into `<ng-content>`.

------------------------------------------------------------------------

## Key Takeaways

-   Uses `<ng-content>`
-   Makes components reusable
-   Allows dynamic external content
-   Does not modify projected content structure

------------------------------------------------------------------------

# Single-slot Projection

## Definition

Single-slot projection uses one `<ng-content>` tag.

All external content is inserted into that single placeholder.

------------------------------------------------------------------------

## Analogy

Imagine a single drawer.

Whatever you put in goes into that one drawer.

------------------------------------------------------------------------

## Practical Example & Usage

### Component

``` ts
@Component({
  selector: 'app-panel',
  template: `
    <div class="panel">
      <ng-content></ng-content>
    </div>
  `
})
export class PanelComponent {}
```

### Usage

``` html
<app-panel>
  <p>Projected paragraph</p>
  <button>Click Me</button>
</app-panel>
```

Both elements go into the same `<ng-content>` slot.

------------------------------------------------------------------------

## Key Takeaways

-   Only one `<ng-content>`
-   All content goes into same slot
-   Simple and clean
-   Useful for basic reusable containers

------------------------------------------------------------------------

# Multi-slot Projection

## Definition

Multi-slot projection allows multiple `<ng-content>` elements with
`select` attributes.

It lets you control where specific content should appear.

------------------------------------------------------------------------

## Analogy

Imagine a wardrobe with labeled sections:

-   Shirts section
-   Pants section
-   Shoes section

Items go into specific sections based on labels.

------------------------------------------------------------------------

## Practical Example & Usage

### Component

``` ts
@Component({
  selector: 'app-layout',
  template: `
    <div class="layout">
      <header>
        <ng-content select="[header]"></ng-content>
      </header>

      <main>
        <ng-content></ng-content>
      </main>

      <footer>
        <ng-content select="[footer]"></ng-content>
      </footer>
    </div>
  `
})
export class LayoutComponent {}
```

### Usage

``` html
<app-layout>
  <div header>Header Content</div>

  <p>Main Content Area</p>

  <div footer>Footer Content</div>
</app-layout>
```

Now content is placed in specific slots based on attributes.

------------------------------------------------------------------------

## Key Takeaways

-   Uses multiple `<ng-content>`
-   `select` attribute defines target slot
-   Enables structured reusable layouts
-   More flexible than single-slot projection
