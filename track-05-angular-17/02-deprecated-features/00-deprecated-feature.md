# Deprecated Features (Angular 17)

## List the Items

Angular 17 does not introduce strict removals, but strongly discourages older patterns in favor of modern syntax and architecture.

1. Structural Directives (*ngIf, *ngFor, NgSwitch) for control flow
2. NgModule-centric architecture (discouraged)
3. Heavy reliance on Zone.js (future shift)

---

# 1. Structural Directives (*ngIf, *ngFor, NgSwitch)

## Reason

Angular has not removed *ngIf or *ngFor in Angular 17 — they are still core features.

- Angular’s new Signals reactivity system makes DOM updates more predictable and fine‑grained.

- Signals + new template syntax could eventually replace structural directives with more explicit, reactive APIs.

- NgSwitch works with three parts:

1. *ngSwitch → container directive.

2. *ngSwitchCase → matches a specific value.

3. *ngSwitchDefault → fallback when no case matches.

### Old:

````html

<div *ngIf="isLoggedIn">Welcome back!</div>

<ul>
  <li *ngFor="let user of users">{{ user.name }}</li>
</ul>

<div [ngSwitch]="status">
  <p *ngSwitchCase="'active'">Active</p>
  <p *ngSwitchCase="'inactive'">Inactive</p>
  <p *ngSwitchDefault>Unknown</p>
</div>

````

### New:


````html
@if (isLoggedIn()) {
  <div>Welcome back!</div>
}

<ul>
  @for (user of users(); track user.name) {
    <li>{{ user.name }}</li>
  }
</ul>

@switch (status()) {
  @case ('active') {
    <p>Active</p>
  }
  @case ('inactive') {
    <p>Inactive</p>
  }
  @default {
    <p>Unknown</p>
  }
}


````

---

## 2. NgModule based Architecture (Deprecated)

NgModules were the traditional way to organize Angular applications.

They grouped components, directives, pipes, and services into cohesive units (AppModule, SharedModule, FeatureModule).

### Reason

1. Standalone APIs (introduced in Angular 14 and enhanced in Angular 16/17) make modules optional.

2. NgModules added extra complexity: developers had to remember declarations, imports, exports, and providers.

3. Standalone components are simpler: you declare dependencies directly in the component, not in a separate module.

---

# 3  Zone.js Dependency

Zone.js is a library Angular has historically relied on to patch async APIs (like setTimeout, Promise, DOM events) and automatically trigger change detection.

## Reason

Angular is moving toward signal-based and zone-less change detection.

 

### Old way:

````ts
@Component({
  selector: 'app-counter',
  template: `{{ count }}`
})
export class CounterComponent {
  count = 0;

  ngOnInit() {
    setTimeout(() => {
      this.count++; // Zone.js patches setTimeout, triggers change detection
    }, 1000);
  }
}

````

#### Here,

- Zone.js automatically detects the async update and refreshes the template.

### New way:

````ts
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `{{ count() }}`
})
export class CounterComponent {
  count = signal(0);

  ngOnInit() {
    setTimeout(() => {
      this.count.update(c => c + 1); // Signals trigger view update directly
    }, 1000);
  }
}

````

No Zone.js needed.

Signals handle reactivity explicitly.

Change detection is predictable and fine‑grained.

### Key Notes

- These are discouraged patterns, not fully removed
- Existing code continues to work
- New syntax is recommended for new development
- Migration can be gradual

---

### Summary

Angular 17 focuses on:

- Cleaner template syntax
- Standalone architecture
- Future zone-less Angular

This improves readability, performance, and developer experience.
