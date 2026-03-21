# NEW FEATURES (Angular 16)

## List the Features

Angular 16 is a major release introducing a new reactivity model and improving developer experience.

Key features:

1. Signals (New Reactivity System)
2. Route data with @Input()
3. Required Inputs
4. DestroyRef API
5. Self‑closing tag updates
6. takeUntilDestroyed() (RxJS Interop)
7. Standalone APIs Enhancements
8. Server-Side Rendering (SSR) Improvements
9. Zone-less Angular (Experimental)

---

# 1. SIGNALS (NEW REATIVITY)

### Before Signals The Problem

We are going to build a Cart Component. It has two fields:

- Product Name (Simple string input).

- Product Price (Simple number input).

It has one derived field:

Total Price (Uses a standard TypeScript get getter).

The problem is that changing the Product Name causes the Total Price getter to run every single time change detection triggers, even though the price didn't change!

### 1. The Component (cart-naive.component.ts)

We will use NgModel and console.count() to show the re-rendering problem in the console.

````ts
@Component({
  selector: 'app-app-card',
  templateUrl: './app-card.component.html',
  styleUrls: ['./app-card.component.css'],
})
export class AppCardComponent {
  productName: string = 'Angular Hoodie';
  unitPrice: number = 29.99;

  
  get totalPrice(): number {
    console.count('Naive Getter Called');
    return this.unitPrice * 10;
  }
}

````


### Template (app.card.component.html)

````html
<div class="p-4 border rounded">
    <h3>Visualizing Naive Change Detection</h3>

    <div class="mb-3">
        <label>Product Name (Does not affect total):</label>
        <input type="text" [(ngModel)]="productName" class="form-control"
            placeholder="Change me...">
    </div>

    <div class="mb-3">
        <label>Unit Price (Affects total):</label>
        <input type="number" [(ngModel)]="unitPrice" class="form-control">
    </div>

    <hr>

    <div class="alert alert-info">
        <p><strong>Product Name:</strong> {{ productName }}</p>
        <p><strong>Unit Price:</strong> {{ unitPrice | currency }}</p>

        <p class="text-danger fw-bold">
            Calculated Total (Naive Getter): {{ totalPrice | currency }}
        </p>
    </div>
</div>
````

### Output

<img width="1017" height="973" alt="image" src="https://github.com/user-attachments/assets/6a2e5801-816b-4b26-804e-be96b3f12020" />


## Here:

- The totalPrice getter is re-calculating (or re-rendering) dozens of times per second, even though the unitPrice never changed.

- The browser is being asked to re-render the Total Price DOM node unnecessarily.

- In a massive grid or complex dashboard, this is the #1 cause of poor performance.

---

# SIGNAL() 

To fix the "unnecessary re-computation" problem we saw in the previous example, we will rewrite the component using Angular 16 Signals.

In this version, we replace standard properties with signal() and the getter with computed(). The result is a "smart" component that only recalculates the total when the price or quantity actually changes—typing in the "Product Name" field will no longer trigger the calculation.


### The Optimized Signal Component 

```ts
@Component({
  selector: 'app-app-card',
  templateUrl: './app-card.component.html',
  standalone: true,
  imports: [FormsModule,CommonModule],
  styleUrls: ['./app-card.component.css'],
})
export class AppCardComponent {
  productName = signal('Angular Hoodie');
  unitPrice = signal(29.99);

  totalPrice = computed(() => {
    console.count("Computed calculating total Price...")
    return this.unitPrice() * 10;
  });

  constructor(){
    effect(() => {
      console.log('Current Product Name is : ' + this.productName()) //Optional
    });
  }
}

```

### Template (app.card.component.html)

````html

<div class="p-4 border rounded">
    <h3>Visualizing Naive Change Detection</h3>

    <div class="mb-3">
        <label>Product Name (Does not affect total):</label>
        <input type="text" [ngModel]="productName()" (ngModelChange)="productName.set($event)" class="form-control"
            placeholder="Change me...">
    </div>

    <div class="mb-3">
        <label>Unit Price (Affects total):</label>
        <input type="number" [ngModel]="unitPrice()" (ngModelChange)="unitPrice.set($event)" class="form-control">
    </div>

    <hr>

    <div class="alert alert-info">
        <p><strong>Product Name:</strong> {{ productName() }}</p>
        <p><strong>Unit Price:</strong> {{ unitPrice()  }}</p>

        <p class="text-danger fw-bold">
            Calculated Total (Naive Getter): {{ totalPrice()  | currency }}
        </p>
    </div>
</div>

````


### Output

<img width="991" height="983" alt="image" src="https://github.com/user-attachments/assets/8092574f-2bfa-41d5-aa48-1144e091e621" />


### Visualizing the Difference in the Browser

When you run this code

1. Initial Load: You see Computed calculating total Price... once.

2. Type in "Product Name": You will see the effect log update the name, but you will NOT see the "Computed Total" log. The total is "memoized" (cached).

3. Change "Unit Price": You will immediately see Computed calculating total Price... because the dependency (unitPrice) changed.


### Angular Signals Overview

| Feature   | How it works in this example                                                                 |
|-----------|-----------------------------------------------------------------------------------------------|
| **signal()**   | Replaces standard variables. You update it using `.set()` or `.update()`.                     |
| **computed()** | The “Problem Solver.” It tracks which signals are used inside it. If you don’t call `productName()` inside the computed block, it knows it doesn’t need to re-run when the name changes. |
| **effect()**   | Useful for logging, manual DOM operations, or analytics. It stays in sync automatically.      |

---

# 2. ROUTE DATA WITH @Input()

## Definition

Router Input Binding is a feature that automatically maps route information—including Path Parameters, Query Parameters, and Resolved Data—directly to properties decorated with @Input() in your component. Instead of manually extracting data from the ActivatedRoute service, the Angular Router "pushes" the values into your component inputs for you.

## Why it was Born (What it replaces)

Before Angular 16, accessing a simple ID from a URL (like /products/5) required a significant amount of boilerplate code.

What it Replaces: 

- It replaces the need to inject the ActivatedRoute service and subscribe to its observables (paramMap, queryParamMap, or data).

- The Problem: * Boilerplate: You had to write ngOnInit logic just to subscribe and unsubscribe.

- Complexity: Subscriptions required manual cleanup or pipe-handling (like takeUntilDestroyed).

- Coupling: Your component became "tightly coupled" to the router, making it harder to reuse as a standard child component elsewhere.

## Example: Fixing the "ActivatedRoute Subscription" Problem

1. The "Before" Problem (The Old Way)

### Product-list.component.ts

````ts
@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule],
  template: `
    <h2>Binding Id value from Product Component</h2>

    <p>Retrieved Id Value is {{ id }}</p>
  `,
})
export class ProductListComponent {
  id!: string | null;

  private router = inject(ActivatedRoute);

  constructor() {
    console.log('Getting id value');

    this.router.paramMap.subscribe((params) => {
      this.id = params.get('id');
    });
  }
}
````

### Product.component.ts

In this scenario, we have to inject the service and subscribe just to get an ID. This is 10+ lines of code for one variable.

````ts
@Component({
  selector: 'app-products',
  standalone: true,
  imports: [CommonModule, AppRoutingModule],
  template: `
    <ul>
      <li *ngFor="let product of Products">
        <a [routerLink]="['/products', product.id]">{{ product.name }}</a>
      </li>
    </ul>
  `,
})
export class ProductsComponent implements OnInit {
  Products: any[] = [];

  ngOnInit(): void {
    this.Products = [
      { id: 1, name: 'Naresh', isPresent: true },
      { id: 2, name: 'Vishanth', isPresent: false },
      { id: 3, name: 'Karthick', isPresent: true },
    ];
  }
}
````

### Output

<img width="634" height="310" alt="image" src="https://github.com/user-attachments/assets/0eb2dca0-171f-42d7-b095-042c4ceb839c" />

#

### 2. The "After" Fix (The Angular 16 Way)

First, you enable the feature in your app.config.ts or main.ts:

````ts
const routes: Routes = [
  { path: 'product', component: ProductsComponent },
  { path: 'products/:id', component: ProductListComponent },
];

bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes, withComponentInputBinding())],
}).catch((err) => console.error(err));

````

### Product.list.component.ts

Now, your component is clean. Angular sees the :id in the route path and looks for an @Input() with the one name:

````ts
@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule],
  template: `
    <h2>Binding Id value from Product Component</h2>

    <p>Retrieved Id Value is {{ id }}</p>
  `,
})
export class ProductListComponent {
  @Input() id!: string;
}
````

### Output

<img width="622" height="286" alt="image" src="https://github.com/user-attachments/assets/f4b6257f-a91a-48b5-bd4a-ab107725bd15" />


## Keynotes

Multi-Source Binding: This works for everything:

1. Path Params: /product/:id -> @Input() id

2. Query Params: ?search=phone -> @Input() search

3. Static Data: { data: { title: 'Home' } } -> @Input() title

Name Matching: The @Input() property name must exactly match the key used in your route definition.


# REQUIRED INPUTS

### DEFINATION

Required Inputs are component or directive properties marked with the { required: true } metadata inside the @Input decorator. When an input is marked as required, the Angular compiler will throw an error if the parent component attempts to use that selector without binding a value to that specific property.

### Why it was Born (What it replaces)

Before Angular 16, all @Input() properties were technically optional. This led to several "silent" failures and developer frustrations:

What it Replaces: 

- It replaces manual runtime validation logic (like if (!this.data) throw Error(...)) and the "non-null assertion operator" (!) which only tricked TypeScript but didn't stop the app from breaking.

1. Silent Bugs: A developer might use a <user-card> but forget to pass the user object. The app would run, but the UI would be blank or throw "Cannot read property of undefined" errors in the console.

2. Opaque APIs: When using someone else's component, you had to check the documentation or the code to know which inputs were actually mandatory.

3. Boilerplate: You often had to write defensive code in ngOnInit to ensure data was present before running logic.

### Example (The "Before" Problem (The Unsafe Way)

1. Create the Child Component (The "Unsafe" Way)

In this component, we use the ! (non-null assertion) to tell TypeScript "don't worry, the data will be there." But we don't actually force the parent to provide it.

### user.card.component.ts

````ts
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div style="border: 2px solid red; padding: 10px; margin: 10px;">
      <h4>User Profile</h4>

      <p>Name : {{ Users.name }}</p>
      <p>Role : {{ Users.role }}</p>
    </div>
  `,
})
export class UserCardComponent {
  @Input() Users!: User;
}

````

### Parent Component (app.component.ts)

Here, we deliberately "forget" to bind the [user] property to the component.

````ts
@Component({
  selector: 'app-root',
  template: `
  <app-user-card></app-user-card>
  `,
  standalone:true,
  imports: [  UserCardComponent]
})
export class AppComponent {
  title = 'angular-16';
}
````

### Output

<img width="1343" height="716" alt="image" src="https://github.com/user-attachments/assets/a39873db-722f-4f3e-a5f0-27ced2206f4d" />


- The Screen: You will likely see a blank white page or only the "Admin Dashboard" header.

- The Console (F12): You will see a massive red error:

## The Solution (Required Input)

If you simply change the child component to:

````ts
export class UserCardComponent {
  @Input({required:true}) Users!: User;
}
````

### Result

<img width="1060" height="536" alt="image" src="https://github.com/user-attachments/assets/4c499782-8087-4c1b-a775-b4ee7c7413ca" />

The Angular Compiler would immediately stop the build and tell you exactly which line in app.component.ts is missing the data. It turns a Runtime Crash into a Compile-time Warning.

---


# 4. DESTROYREF API

### Definition

DestroyRef is an injectable service that allows you to register "destroy callbacks" at any point during a component, directive, or pipe's lifecycle. It provides an onDestroy() method that takes a function to be executed when the surrounding "injection context" (usually the component) is destroyed.

1. Unlike ngOnDestroy, which is a method you must define on a class.

2. DestroyRef is a service you inject and use wherever you need it.

### Why it was Born (What it replaces)

Before Angular 16, the only way to clean up resources (like unsubscribing from an Observable or clearing a setInterval) was the ngOnDestroy lifecycle hook.

What it Replaces: It replaces the mandatory use of the OnDestroy interface and the ngOnDestroy() class method.

### Example: Fixing the "Subscription Leak" Problem

1. The "Before" Problem (The Boilerplate Way)

In this version, we have to keep a reference to the subscription and remember to kill it in a separate method. If we forget implements OnDestroy, the code fails silently and leaks memory.

### Data Component

```ts
@Component({
  selector: 'app-data',
  standalone: true,
  imports: [CommonModule],
  template: ` <p>Data Component is Works!</p> `,
})
export class DataComponent implements OnInit, OnDestroy {
  subscription!: Subscription;

  ngOnInit(): void {
    this.subscription = interval(1000).subscribe((val) => {
      console.log(val);
    });
  }
  ngOnDestroy(): void {
    this.subscription.unsubscribe();
  }
}
```

### 2. The "After" Fix (The Angular 16 Way)

With DestroyRef, we can keep the "start" and "stop" logic together in the same place. This is especially powerful when using the new takeUntilDestroyed operator.

````ts
import {
  Component,
  DestroyRef,
  inject
} from '@angular/core';
import { CommonModule } from '@angular/common';
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';
import { interval, Subscription } from 'rxjs';

@Component({
  selector: 'app-data',
  standalone: true,
  imports: [CommonModule],
  template: ` <p>Data Component is Works!</p> `,
})
export class DataComponent {
  subscription!: Subscription;

  constructor() {
    const destroyRef = inject(DestroyRef);

    interval(1000)
      .pipe(takeUntilDestroyed())
      .subscribe((val) => console.log(val));

    destroyRef.onDestroy(() => {
      console.log('Destroyed the component....');
    });
  }
}

`````

### Ouptut

<img width="957" height="731" alt="image" src="https://github.com/user-attachments/assets/18a16a9f-4846-4edf-9ba9-92aa01fa4dc0" />

### Keynotes

-  Multiple Callbacks: You can call destroyRef.onDestroy() multiple times to register different cleanup tasks. They will execute in the order they were registered.

- RxJS Integration: The takeUntilDestroyed pipe operator is the most common use case, drastically reducing the lines of code needed for safe subscriptions.

---

# SELF-CLOSING TAG UPDATES

### Definition

Self-Closing Tags allow you to use a single tag ending in /> for Angular components that do not have any nested content (projected content). Previously, even if a component was empty, you were strictly required to write both an opening and a closing tag.


### Example

````ts
@Component({
  selector: 'app-root',
  template: `
    <div class="toolbar">
      <app-logo />
    </div>

    <router-outlet></router-outlet>
  `,
  standalone: true,
  imports: [RouterOutlet, LogoComponent],
})
export class AppComponent {
  title = 'angular-16';
}
````

---

### Why introduced

- Replace manual lifecycle cleanup
- Avoid overuse of ngOnDestroy

## Key Notes

- Cleaner lifecycle handling
- Works with standalone APIs

---

# 3. takeUntilDestroyed()

## Example

```ts
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';

this.http.get('/api')
  .pipe(takeUntilDestroyed())
  .subscribe();
```

### Why introduced

- Simplify subscription management
- Prevent memory leaks

## Key Notes

- No manual unsubscribe needed
- Cleaner RxJS integration

---

# 4. Required Inputs

## Example

```ts
@Input({ required: true })
user!: string;
```

### Why introduced

- Ensure required inputs are passed
- Prevent runtime errors

## Key Notes

- Compile-time validation
- Improves reliability

---

# 5. Standalone APIs Enhancements

## Example

```ts
bootstrapApplication(AppComponent);
```

### Why introduced

- Remove dependency on NgModules
- Simplify Angular apps

## Key Notes

- NgModules optional
- Cleaner structure

---

# 7. SSR Improvements

## Example

Improved hydration support

### Why introduced

- Faster loading
- Better SEO

## Key Notes

- Better Angular Universal support

---

# 8. Zone-less Angular (Experimental)

## Example

Signal-driven updates without Zone.js

### Why introduced

- Improve performance
- Reduce unnecessary change detection

## Key Notes

- Experimental
- Future direction

---

# Summary

Angular 16 introduces:

- Signals
- Better lifecycle APIs
- Improved RxJS interop
- Stronger standalone architecture

This version marks the shift toward modern Angular.
