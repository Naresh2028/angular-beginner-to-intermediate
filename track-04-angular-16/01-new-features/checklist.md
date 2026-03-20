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

