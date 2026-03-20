# DEPRECATED FEATURES - ANGULAR 16

## List All the Deprecated Features

Angular 16 continues moving toward **modern, standalone, and signal-based architecture**. Some legacy patterns are discouraged or deprecated.

Main areas:

1. NgModule-heavy patterns (discouraged)
2. Class-based lifecycle-heavy RxJS cleanup patterns
3. Zone.js reliance (gradual shift away)
4. Legacy injection patterns
5. Old RxJS subscription management

---

# 1. NgModule-heavy Architecture (Discouraged)

## Reason

Angular is moving toward **Standalone APIs**.

### Example

❌ Old

@NgModule({
  declarations: [AppComponent]
})
export class AppModule {}

✅ New

bootstrapApplication(AppComponent)

---

# 2. Manual Subscription Cleanup (Deprecated Pattern)

## Reason

Angular 16 introduces **takeUntilDestroyed()** and **DestroyRef**.

### Example

❌ Old

ngOnDestroy() {
  this.subscription.unsubscribe();
}

✅ New

observable.pipe(takeUntilDestroyed()).subscribe()

---

# 3. Zone.js Dependency (Reducing Importance)

## Reason

Angular is moving toward **zone-less applications** using signals.

### Example

❌ Old

Relies heavily on Zone.js change detection

✅ New

Signals-driven reactivity (no zone dependency)

---

# 4. Constructor-only Injection Pattern

## Reason

Angular promotes `inject()` function.

### Example

❌ Old

constructor(private service: MyService) {}

✅ New

const service = inject(MyService);

---

# 5. Imperative State with RxJS (Discouraged in Some Cases)

## Reason

Signals provide simpler state management.

### Example

❌ Old

subject.next(value)

✅ New

signal.set(value)

---

# Key Notes

- Angular 16 introduces **Signals** (major shift)
- RxJS is still important but not always required
- Standalone APIs are the future
- Zone-less Angular is emerging
- Migration should be gradual

---

# Summary

Angular 16 deprecations focus on:

- Moving away from NgModules
- Reducing manual lifecycle cleanup
- Encouraging signals over complex RxJS
- Preparing for zone-less Angular

This version marks a transition toward **simpler and more reactive Angular architecture**.
