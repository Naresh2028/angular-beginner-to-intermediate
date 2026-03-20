# Migration Notes (Angular 15 → Angular 16)

## How can I transform my Angular 15 project into Angular 16?

Follow these steps to upgrade your Angular 15 project to Angular 16 safely.

---

## 1. Verify Current Version

```
ng version
```

Ensure your project is using Angular 15.

---

## 2. Update Angular CLI (Global)

```
npm uninstall -g @angular/cli
npm install -g @angular/cli@16
```

---

## 3. Update Angular Core & CLI

Inside your project:

```
ng update @angular/core@16 @angular/cli@16
```

This will:
- Update Angular packages
- Update peer dependencies
- Adjust config files

---

## 4. Update Angular Material (If Used)

```
ng update @angular/material@16
```

---

## 5. Update Required Dependencies

Angular 16 requires:

```
TypeScript ~5.0
RxJS ~7.8
Node 16+
```

Run:

```
npm install
```

---

## 6. Test the Application

```
ng serve
```

Check:

- No build errors
- No console errors
- All features working

---

## 7. Optional: Start Using Signals (NEW)

Angular 16 introduces **Signals** (new reactivity system).

Example:

```
import { signal } from '@angular/core';

const count = signal(0);
count.set(1);
```

---

## 8. Optional: DestroyRef & takeUntilDestroyed

Better cleanup for subscriptions.

Example:

```
import { takeUntilDestroyed } from '@angular/core/rxjs-interop';
```

---

## 9. Optional: Standalone APIs (Recommended)

If not already used, consider migrating to:

```
bootstrapApplication()
provideRouter()
provideHttpClient()
```

---

# Key Notes

- Upgrade one version at a time
- Always commit before upgrade
- Use `ng update`, not manual edits
- Angular 16 introduces Signals (big shift)
- No need to rewrite everything immediately
- Gradual adoption is best

---

# Summary

Angular 16 introduces:

- Signals (new reactivity model)
- Improved performance
- Better RxJS interop
- Cleaner architecture

This version marks a major shift toward **modern reactive Angular**.

