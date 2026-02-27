# Directives

## Definition

Directives in Angular are special instructions in the template that
modify the structure or behavior of DOM elements.

They allow you to:

-   Change appearance
-   Control rendering
-   Attach custom behavior to elements

Directives enhance HTML with dynamic capabilities.

------------------------------------------------------------------------

## Analogy

Think of traffic signals on a road.

-   They control movement.
-   They change behavior based on conditions.

Directives control how elements behave and appear in the UI.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Conditionally Displaying Content

``` html
<p *ngIf="isLoggedIn">Welcome Back!</p>
```

Here:

-   The paragraph is rendered only if `isLoggedIn` is true.
-   Angular modifies the DOM structure dynamically.

------------------------------------------------------------------------

## Types

Angular provides three main types of directives:

1.  Built-in Directive
2.  Structural Directives
3.  Attribute Directives

------------------------------------------------------------------------

# Built-in Directives

## Definition

Built-in directives are directives provided by Angular out of the box.

They help manipulate DOM structure and appearance without writing custom
logic.

Common built-in directives:

-   `*ngIf`
-   `*ngFor`
-   `ngSwitch`
-   `[ngClass]`
-   `[ngStyle]`

------------------------------------------------------------------------

## Analogy

Think of built-in tools in a toolkit.

-   You don't build them yourself.
-   You use them when needed.

Angular provides these directives ready to use.

------------------------------------------------------------------------

## Production-Level Example

### 1️⃣ \*ngFor (Rendering Lists)

``` html
<ul>
  <li *ngFor="let product of products">
    {{ product.name }}
  </li>
</ul>
```

Used for rendering dynamic lists from API responses.

------------------------------------------------------------------------

### 2️⃣ ngClass (Dynamic Styling)

``` html
<div [ngClass]="{ 'active': isActive }">
  Status Box
</div>
```

Used for applying conditional CSS classes.

------------------------------------------------------------------------

### 3️⃣ ngSwitch (Conditional Rendering)

``` html
<div [ngSwitch]="role">
  <p *ngSwitchCase="'admin'">Admin Panel</p>
  <p *ngSwitchCase="'user'">User Dashboard</p>
  <p *ngSwitchDefault>Guest View</p>
</div>
```

Used for role-based UI rendering.

------------------------------------------------------------------------

## Key Notes

-   Directives extend HTML functionality.
-   Structural directives change DOM structure (`*ngIf`, `*ngFor`).
-   Attribute directives change appearance or behavior (`ngClass`,
    `ngStyle`).
-   Built-in directives are optimized and production-ready.
-   Custom directives can be created for advanced behavior.
