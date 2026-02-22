# Dynamic Component Loader

## Definition

Dynamic Component Loading in Angular allows you to create and render
components at runtime instead of declaring them directly in the
template.

It is useful when:

-   The component to render depends on user action
-   You want to load components conditionally
-   You are building modals, dialogs, dashboards, or plugin systems

Angular provides `ViewContainerRef` to dynamically create components.

------------------------------------------------------------------------

## Analogy

Think of a stage manager in a theater.

Instead of placing all actors on stage at once:

-   The manager calls actors only when their scene begins.
-   Different actors appear based on the script.

Dynamic component loading works the same way ---\
components appear only when needed.

------------------------------------------------------------------------

## Practical Example & Usage

### Step 1: Create a Component to Load

``` ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-dynamic-child',
  template: `<p>Dynamically Loaded Component Works!</p>`
})
export class DynamicChildComponent {}
```

------------------------------------------------------------------------

### Step 2: Host Component

``` ts
import { Component, ViewChild, ViewContainerRef } from '@angular/core';
import { DynamicChildComponent } from './dynamic-child.component';

@Component({
  selector: 'app-host',
  template: `
    <button (click)="loadComponent()">Load Component</button>
    <ng-template #container></ng-template>
  `
})
export class HostComponent {

  @ViewChild('container', { read: ViewContainerRef })
  container!: ViewContainerRef;

  loadComponent() {
    this.container.clear();
    this.container.createComponent(DynamicChildComponent);
  }
}
```

------------------------------------------------------------------------

### What Happens?

-   When the button is clicked
-   Angular creates the component dynamically
-   It gets inserted into the `ng-template` location

------------------------------------------------------------------------

## Key Takeaways

-   Used for runtime component creation
-   Uses `ViewContainerRef`
-   Ideal for modals, dashboards, plugin systems
-   Improves flexibility
-   Avoid overusing --- keep architecture clean
