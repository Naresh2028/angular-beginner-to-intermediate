
# DEPRECATED FEATURES (Angular 14)

## List the Features

Angular 14 deprecated a few APIs and patterns to simplify the framework and prepare for newer architectures.

Common deprecated areas include:

1. Legacy `ComponentFactoryResolver` usage
2. `AbstractControlOptions` legacy patterns
3. `TestBed.get()` method
4. Some legacy Angular compiler options
5. Older View Engine related APIs

---

# 1. ComponentFactoryResolver (Direct Usage)

## Reason

Angular introduced simpler dynamic component creation using `ViewContainerRef.createComponent()`.
The older pattern required `ComponentFactoryResolver`, which added unnecessary complexity.

## Key Notes

- Modern Angular uses simplified APIs.
- Recommended approach:

```ts
viewContainerRef.createComponent(MyComponent);
```

- Direct dependency on `ComponentFactoryResolver` is discouraged.

---

# 2. Legacy Form Typing Patterns

## Reason

Angular 14 introduced **Typed Reactive Forms**, which replaced the loosely typed form approach.

Older usage:

```ts
const control = new FormControl('');
```

Recommended typed approach:

```ts
const control = new FormControl<string>('');
```

## Key Notes

- Improves type safety.
- Prevents runtime form errors.
- Recommended for all new Angular applications.

---

# 3. TestBed.get()

## Reason

Angular deprecated `TestBed.get()` in favor of the more type-safe `TestBed.inject()`.

Deprecated usage:

```ts
const service = TestBed.get(MyService);
```

Recommended usage:

```ts
const service = TestBed.inject(MyService);
```

## Key Notes

- `inject()` provides better typing.
- Aligns with Angular’s modern dependency injection improvements.

---

# 4. View Engine Related APIs

## Reason

Angular officially moved away from **View Engine** and standardized on the **Ivy rendering engine**.

View Engine APIs were deprecated because Ivy is:

- Faster
- Smaller bundle size
- Better debugging support

## Key Notes

- Ivy became the default Angular renderer.
- Most View Engine configuration options are no longer required.

---

# 5. Legacy Compiler Configuration Options

## Reason

Some compiler flags used during View Engine era became unnecessary after Ivy adoption.

Examples include certain `angularCompilerOptions` flags.

## Key Notes

- Angular CLI automatically manages modern compiler settings.
- Developers rarely need manual compiler configuration.

---

# Summary

Angular 14 deprecated features mainly to:

- Encourage **typed APIs**
- Simplify **dynamic component creation**
- Remove **View Engine remnants**
- Improve **developer experience**

Key direction of Angular 14:

- Strong typing
- Simpler APIs
- Ivy-first architecture

