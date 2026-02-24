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

### Child Component

``` ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-card',
  template:`
  <div class="card">
    <ng-content></ng-content>
  </div>
  `,
  styles: [`
    .card {
      border: 2px solid #3498db;
      border-radius: 8px;
      padding: 20px;
      margin: 10px;
      max-width: 300px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      background-color: #f9f9f9;
    }
  `]
})
export class CardComponent implements OnInit {
}

```

### Parent Component

``` ts
import { AfterViewChecked, AfterViewInit, Component, OnDestroy, OnInit, ViewChild } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
  <router-outlet></router-outlet>
  <h1>App Component</h1>
  <app-card>
    <h3>User Profile</h3>
    <p>Naresh</p>
    <button class="btn btn-primary" (click)="sendRequest()">Click Me</button>
  </app-card>
`
})
export class AppComponent {

  sendRequest(){
    alert("Clicked Me")
  }
  
}

```

### Output

<img width="655" height="413" alt="image" src="https://github.com/user-attachments/assets/4448421d-cf46-408d-9ff6-7885c20a9197" />


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

<img width="3840" height="3840" alt="image" src="https://github.com/user-attachments/assets/97482e55-7aff-442d-8fda-5a039e1f70af" />


Items go into specific sections based on labels.

------------------------------------------------------------------------

## Scenario to Use

Use multi-slot projection when:

-   Building layouts with header, body, footer
-   Designing complex reusable UI components
-   You need structured content placement

------------------------------------------------------------------------

## Practical Example

### Child Component

``` ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-layout',
  template: `
  <div class="dashboard-card">
  <header class="card-header">
    <ng-content select="[header]">

    </ng-content>
  </header>

  <main class="card-body">
  <ng-content></ng-content>
  </main>

  <footer class="card-footer">
    <ng-content select="[footer]">

    </ng-content>
  </footer>
  </div>
  `,
  styles: [`
    .dashboard-card { border: 1px solid #ddd; border-radius: 8px; width: 400px; overflow: hidden; }
    .card-header { background: #f4f4f4; padding: 15px; border-bottom: 1px solid #ddd; font-weight: bold; }
    .card-body { padding: 20px; min-height: 100px; }
    .card-footer { background: #fafafa; padding: 10px; border-top: 1px solid #ddd; text-align: right; }
  `]
})
export class LayoutComponent implements OnInit {

  constructor() { }

  ngOnInit(): void {
  }

}

```

### Parent Component

``` ts
import { AfterViewChecked, AfterViewInit, Component, OnDestroy, OnInit, ViewChild } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
  <router-outlet></router-outlet>
  <h1 class="text-center">Angular Learning</h1>
  <app-layout>
    
    <div footer>
      <p>© Parewa Labs Pvt. Ltd. All rights reserved.</p>
    </div>  
    <div header>
      <p><strong>User Name :</strong> Naresh</p>
    </div> 
    

    <p><strong>Email:</strong> admin@company.com</p>
      <p><strong>Last Login:</strong> Feb 24, 2026</p>
      <p>This user has full access to the system settings.</p>
    
  </app-layout>
`
})
export class AppComponent {

  sendRequest() {
    alert("Clicked Me")
  }

}

```

<img width="1279" height="585" alt="image" src="https://github.com/user-attachments/assets/1553f62b-b594-473f-b5df-f40185afb04f" />


Content is projected into specific slots.

### 1. Select attribute behaviour

The select attribute acts like a filter or a magnet. When you define <ng-content select="[header]">, you are telling the child component: "Only pull in content from the parent that has the attribute 'header'."

- Targeting: It uses standard CSS selectors (attributes, classes, or tags) to identify specific pieces of content.
- clusion: Any content that does not match the selector is ignored by that specific slot and moves on to find the next available slot or the default "catch-all" slot.

### 2. Placement Irrespective of Order

In multi-slot projection, the physical order of the code in the Parent component does not matter. The Child's template is the boss and determines the final layout.

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
<img width="1155" height="313" alt="image" src="https://github.com/user-attachments/assets/88842e39-44ac-4dc0-8389-7d1b19e6ca10" />


The projected content appears only if `isVisible` is true.

------------------------------------------------------------------------

## Key Notes

-   `<ng-content>` enables projection
-   `select` attribute enables multi-slot control
-   Structural directives allow conditional projection
-   Improves component reusability and flexibility




