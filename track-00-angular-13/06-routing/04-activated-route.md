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

## Advanced Reactive Version

### product.component.ts (The Product List Component)

``` ts
export class ProductsComponent implements OnInit {
  products: Product[] = [];

  constructor() {}

  ngOnInit(): void {
    this.products = [
      { id: '101', name: 'Angular Pro Laptop', price: 1200 },
      { id: '102', name: 'TypeScript Mechanical Keyboard', price: 150 },
      { id: '103', name: 'RxJS Stream Monitor', price: 300 },
    ];
  }
}
```

### product.component.html (The Product List Component)

``` html
<div>
    <div class="card shadow-sm">
        <div>
            <h3 class="mb-0">Our Product Catalog</h3>

            <ul>
                <li *ngFor="let item of products">
                    <span style="color:red;">Product name :</span> {{item.name}}
                    <p>Product Price {{item.price}}</p>

                    <a [routerLink]="[item.id]">View Product Id</a>
                </li>
            </ul>

        </div>
    </div>
</div>

<router-outlet></router-outlet>

```
### Product-Detail Component

we use the Observable approach to update the productId and simulated data every time the ID in the URL changes.

```ts
@Component({
  selector: 'app-product-detail',
  template:`
  <h3>Product Details</h3>

  <p>Selected Product Id {{this.productId}}</p>

  <button class="btn btn-primary" routerLink="/products">Back To Product</button>
  `
})
export class ProductDetailComponent implements OnInit {

  productId!:string | null;

  constructor(private route:ActivatedRoute) { }

  ngOnInit(): void {
    this.route.paramMap.subscribe(params => {
      this.productId = params.get('id')
    });
  }

}
```

### The Route Configuration

```ts
  {
    path: 'products',
    component: ProductsComponent,
    children: [{ path: ':id', component: ProductDetailComponent }],
  },
```

### Output

<img width="1349" height="886" alt="image" src="https://github.com/user-attachments/assets/be101d46-7cc1-4e01-abcf-9635cbc1bc7b" />

## Angular Route ParamMap Usage

| Feature        | route.snapshot.paramMap         | route.paramMap.subscribe                  |
|----------------|---------------------------------|-------------------------------------------|
| Initial Load   | Works perfectly.                | Works perfectly.                          |
| Same-Page Nav  | Fails to update the UI.         | Updates the UI instantly.                 |
| Complexity     | Simple (no cleanup needed).     | Requires an observable stream.            |
| Best For       | Static detail pages.            | Dashboards, Next/Prev features.           |


Production Use Cases:

-   Dynamic product pages
-   User profile pages
-   Search filters using query parameters
-   Multi-step navigation flows

------------------------------------------------------------------------

