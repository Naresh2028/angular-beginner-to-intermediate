
# Migration Notes (Angular 17 → Angular 18)

## How can I transform my Angular 17 project into Angular 18?

Follow these steps to safely upgrade your Angular 17 project to Angular 18.

---

## 1. Verify Current Version

```
ng version
```

Ensure your project is using Angular 17.

---

## 2. Update Global Angular CLI

```
npm uninstall -g @angular/cli
npm install -g @angular/cli@18
```

---

## 3. Update Angular Core & CLI

Inside your project:

```
ng update @angular/core@18 @angular/cli@18
```

This will:

- Update Angular packages
- Adjust configurations
- Update dependencies

---

## 4. Update Angular Material (If Used)

```
ng update @angular/material@18
```

---

## 5. Update Required Dependencies

Angular 18 typically requires:

```
TypeScript (latest compatible version)
Node 18+
```

Then run:

```
npm install
```

---

## 6. Run the Application

```
ng serve
```

Verify:

- Application compiles successfully
- No runtime errors
- Features working correctly

---

## 7. Optional: Adopt Angular 18 Improvements

Angular 18 continues improvements in:

- Signals ecosystem
- Performance optimizations
- Build tooling

---

## Key Notes

- Upgrade one version at a time
- Always commit before upgrading
- Use `ng update`, avoid manual edits
- Angular 18 focuses on stability and performance
- Existing Angular 17 code works without major changes

---

## Summary

Angular 18 focuses on:

- Performance improvements
- Stability enhancements
- Better developer experience

Migration is smooth with minimal changes required.
