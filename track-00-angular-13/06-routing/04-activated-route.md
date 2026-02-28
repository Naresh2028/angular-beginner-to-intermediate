# ActivatedRoute Service

## Definition

ActivatedRoute is an Angular service that provides access to information
about the current route.

It allows you to:

-   Read route parameters
-   Access query parameters
-   Retrieve route data
-   React to route changes dynamically

It is commonly used inside routed components.

------------------------------------------------------------------------

## Analogy

Think of a delivery package label.

-   The label contains destination details.
-   The receiver reads the label to know where it came from and what it
    contains.

ActivatedRoute provides route-related information to the component.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Product Detail Page

### Routing Configuration

``` ts
const routes: Routes = [
  { path: 'products/:id', component: ProductDetailComponent }
];
```

------------------------------------------------------------------------

### Component Implementation

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-product-detail',
  template: `
    <h2>Product ID: {{ productId }}</h2>
  `
})
export class ProductDetailComponent implements OnInit {

  productId!: string | null;

  constructor(private route: ActivatedRoute) {}

  ngOnInit() {
    this.productId = this.route.snapshot.paramMap.get('id');
  }

}
```

------------------------------------------------------------------------

### Advanced Reactive Version

``` ts
ngOnInit() {
  this.route.paramMap.subscribe(params => {
    this.productId = params.get('id');
  });
}
```

Production Use Cases:

-   Dynamic product pages
-   User profile pages
-   Search filters using query parameters
-   Multi-step navigation flows

------------------------------------------------------------------------

