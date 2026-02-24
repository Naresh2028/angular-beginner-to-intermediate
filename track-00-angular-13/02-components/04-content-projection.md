# Content Projection

## Definition

Content Projection allows you to insert external content into a reusable
Angular component using `<ng-content>`.

It enables flexible and customizable UI components while keeping layout
and structure reusable.

------------------------------------------------------------------------

## Analogy

Think of a reusable container.

-   The container design remains fixed.
-   The items placed inside can change depending on need.

The component provides structure. The projected content provides
flexibility.

------------------------------------------------------------------------

## Types

-   Single-slot content projection
-   Multi-slot content projection
-   Conditional content projection

------------------------------------------------------------------------

# Single-slot content projection

## Definition

Single-slot projection uses one `<ng-content>` tag.

All external content passed between the component tags is projected into
that single placeholder.

------------------------------------------------------------------------

## Analogy

Imagine a single drawer.

Whatever you put inside goes into that one drawer.

------------------------------------------------------------------------

## Scenario to Use

Use single-slot projection when:

-   Building simple reusable containers (cards, panels)
-   You don't need structured placement
-   All content should appear in one area

------------------------------------------------------------------------

## Practical Example

### Component

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
  <p>Description content</p>
</app-card>
```

All content is placed inside the same slot.

------------------------------------------------------------------------

# Multi-slot content projection

## Definition

Multi-slot projection uses multiple `<ng-content>` elements with
`select` attributes to control where content appears.

------------------------------------------------------------------------

## Analogy

Think of a wardrobe with labeled sections:

-   Shirts section
-   Pants section
-   Shoes section

Items go into specific sections based on labels.

------------------------------------------------------------------------

## Scenario to Use

Use multi-slot projection when:

-   Building layouts with header, body, footer
-   Designing complex reusable UI components
-   You need structured content placement

------------------------------------------------------------------------

## Practical Example

### Component

``` ts
@Component({
  selector: 'app-layout',
  template: `
    <header>
      <ng-content select="[header]"></ng-content>
    </header>

    <main>
      <ng-content></ng-content>
    </main>

    <footer>
      <ng-content select="[footer]"></ng-content>
    </footer>
  `
})
export class LayoutComponent {}
```

### Usage

``` html
<app-layout>
  <div header>Header Content</div>
  <p>Main Content</p>
  <div footer>Footer Content</div>
</app-layout>
```

Content is projected into specific slots.

------------------------------------------------------------------------

# Conditional content projection

## Definition

Conditional content projection controls whether projected content is
displayed using Angular structural directives like `*ngIf`.

------------------------------------------------------------------------

## Analogy

Think of a curtain in a theater.

The stage exists, but the curtain determines whether the audience can
see the performance.

------------------------------------------------------------------------

## Scenario to Use

Use conditional projection when:

-   Showing content based on user role
-   Toggling visibility dynamically
-   Rendering optional sections

------------------------------------------------------------------------

## Practical Example

### Component

``` ts
@Component({
  selector: 'app-conditional-card',
  template: `
    <div class="card">
      <ng-content *ngIf="isVisible"></ng-content>
    </div>
  `
})
export class ConditionalCardComponent {
  isVisible = true;
}
```

### Usage

``` html
<app-conditional-card>
  <p>This content appears conditionally.</p>
</app-conditional-card>
```

The projected content appears only if `isVisible` is true.

------------------------------------------------------------------------

## Key Notes

-   `<ng-content>` enables projection
-   `select` attribute enables multi-slot control
-   Structural directives allow conditional projection
-   Improves component reusability and flexibility
