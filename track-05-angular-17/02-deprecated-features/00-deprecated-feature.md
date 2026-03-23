# Deprecated Features (Angular 17)

## List the Items

Angular 17 does not introduce strict removals, but strongly discourages older patterns in favor of modern syntax and architecture.

1. Structural Directives (*ngIf, *ngFor, *ngSwitch) for control flow
2. NgSwitch (legacy approach)
3. Microsyntax-heavy templates
4. NgModule-centric architecture (discouraged)
5. Heavy reliance on Zone.js (future shift)

---

# 1. Structural Directives (*ngIf, *ngFor)

## Reason

Angular 17 introduces new control flow syntax that is more readable and maintainable.

### Example

Old:

<div *ngIf="isVisible">Content</div>

New:

@if (isVisible) {
  <div>Content</div>
}

---

# 2. NgFor (Microsyntax)

## Reason

New @for syntax improves readability and performance.

### Example

Old:

<div *ngFor="let item of items">{{ item }}</div>

New:

@for (item of items; track item) {
  <div>{{ item }}</div>
}

---

# 3. NgSwitch

## Reason

Replaced by cleaner @switch syntax.

### Example

Old:

<div [ngSwitch]="status">
  <div *ngSwitchCase="'success'">Success</div>
</div>

New:

@switch (status) {
  @case ('success') {
    <div>Success</div>
  }
}

---

# 4. NgModule-heavy Architecture

## Reason

Standalone APIs simplify Angular applications.

### Example

Old:

@NgModule({
  declarations: [AppComponent]
})
export class AppModule {}

New:

bootstrapApplication(AppComponent);

---

# 5. Zone.js Dependency

## Reason

Angular is moving toward signal-based and zone-less change detection.

### Example

Old:

Automatic change detection using Zone.js

New:

Signal-driven updates (future direction)

---

# Key Notes

- These are discouraged patterns, not fully removed
- Existing code continues to work
- New syntax is recommended for new development
- Migration can be gradual

---

# Summary

Angular 17 focuses on:

- Cleaner template syntax
- Standalone architecture
- Future zone-less Angular

This improves readability, performance, and developer experience.
