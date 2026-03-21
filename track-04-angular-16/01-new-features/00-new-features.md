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

# 1. Signals (New Reactivity System)

## Before Signals The Problem

We are going to build a Cart Component. It has two fields:

- Product Name (Simple string input).

- Product Price (Simple number input).

It has one derived field:

Total Price (Uses a standard TypeScript get getter).

The problem is that changing the Product Name causes the Total Price getter to run every single time change detection triggers, even though the price didn't change!

## 1. The Component (cart-naive.component.ts)

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


## Template (app.card.component.html)

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

## SIGNAL() Example 

To fix the "unnecessary re-computation" problem we saw in the previous example, we will rewrite the component using Angular 16 Signals.

In this version, we replace standard properties with signal() and the getter with computed(). The result is a "smart" component that only recalculates the total when the price or quantity actually changes—typing in the "Product Name" field will no longer trigger the calculation.


## The Optimized Signal Component 

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

## Template (app.card.component.html)

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


## Visualizing the Difference in the Browser

When you run this code

1. Initial Load: You see 🚀 Computed Total Running... once.

2. Type in "Product Name": You will see the effect log update the name, but you will NOT see the "Computed Total" log. The total is "memoized" (cached).

3. Change "Unit Price": You will immediately see Computed calculating total Price... because the dependency (unitPrice) changed.

### Why introduced

# Angular Signals Overview

| Feature   | How it works in this example                                                                 |
|-----------|-----------------------------------------------------------------------------------------------|
| **signal()**   | Replaces standard variables. You update it using `.set()` or `.update()`.                     |
| **computed()** | The “Problem Solver.” It tracks which signals are used inside it. If you don’t call `productName()` inside the computed block, it knows it doesn’t need to re-run when the name changes. |
| **effect()**   | Useful for logging, manual DOM operations, or analytics. It stays in sync automatically.      |





# 2. DestroyRef API

## Example

```ts
import { DestroyRef, inject } from '@angular/core';

const destroyRef = inject(DestroyRef);

destroyRef.onDestroy(() => {
  console.log('cleanup');
});
```

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
