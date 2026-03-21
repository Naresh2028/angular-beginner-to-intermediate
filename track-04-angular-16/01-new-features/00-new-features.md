# NEW FEATURES (Angular 16)

## List the Features

Angular 16 is a major release introducing a new reactivity model and improving developer experience.

Key features:

1. Signals (New Reactivity System)
2. Computed & Effect APIs
3. DestroyRef API
4. takeUntilDestroyed() (RxJS Interop)
5. Required Inputs
6. Standalone APIs Enhancements
7. Server-Side Rendering (SSR) Improvements
8. Zone-less Angular (Experimental)

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

## Example (Why it was introduced)

```ts
import { signal } from '@angular/core';

const count = signal(0);
count.set(1);
console.log(count());
```

### Why introduced

- Replace simple RxJS state use cases
- Provide fine-grained reactivity
- Improve performance

## Key Notes

- No subscriptions required
- Synchronous updates
- Automatically updates UI



---

# 2. Computed & Effect APIs

## Example

```ts
import { signal, computed, effect } from '@angular/core';

const count = signal(1);

const double = computed(() => count() * 2);

effect(() => {
  console.log(count());
});
```

### Why introduced

- Derived state (computed)
- Side effects (effect)
- Cleaner reactive programming

## Key Notes

- computed → derived values
- effect → side effects
- Works without RxJS

---

# 3. DestroyRef API

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

# 4. takeUntilDestroyed()

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

# 5. Required Inputs

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

# 6. Standalone APIs Enhancements

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
