# Deprecated Features (Angular 18)

## List the Items

Angular 18 continues the shift toward modern, signal-based, standalone architecture.
There are no major hard removals, but several patterns are strongly discouraged.

1. Structural Directives (*ngIf, *ngFor, *ngSwitch)
2. NgModule-centric architecture
3. Zone.js dependency (long-term direction)
4. Overuse of RxJS for local state
5. Complex template microsyntax

---

# 1. Structural Directives (*ngIf, *ngFor, *ngSwitch)

## Reason

Replaced by new control flow syntax (@if, @for, @switch) for better readability and performance.

Example:

Old:
<div *ngIf="isVisible">Content</div>

New:
@if (isVisible) {
  <div>Content</div>
}

---

# 2. NgModule-centric Architecture

## Reason

Standalone APIs simplify Angular apps and reduce boilerplate.

Example:

Old:
@NgModule({
  declarations: [AppComponent]
})
export class AppModule {}

New:
bootstrapApplication(AppComponent);

---

# 3. Zone.js Dependency

## Reason

Angular is moving toward zone-less apps using Signals.

Example:

Old:
Zone.js triggers change detection automatically

New:
Signal-based fine-grained updates

---

# 4. Overuse of RxJS for Local State

## Reason

Signals provide simpler state management for component-level data.

Example:

Old:
const count$ = new BehaviorSubject(0);
count$.next(1);

New:
const count = signal(0);
count.set(1);

---

# 5. Complex Template Microsyntax

## Reason

Microsyntax is harder to read compared to new control flow syntax.

Example:

Old:
<div *ngFor="let item of items">{{ item }}</div>

New:
@for (item of items; track item) {
  <div>{{ item }}</div>
}

---

# Summary

Angular 18 discourages:

- Structural directive microsyntax
- NgModule-heavy architecture
- Zone.js dependency
- Excess RxJS for simple state

The direction is toward:

- Signals
- Standalone APIs
- Cleaner templates
