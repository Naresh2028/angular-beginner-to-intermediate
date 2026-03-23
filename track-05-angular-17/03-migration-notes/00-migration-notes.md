# Migration Notes (Angular 16 → Angular 17)

## How can I transform my Angular 16 project into Angular 17?

Follow these steps to safely upgrade your Angular 16 project to Angular
17.

------------------------------------------------------------------------

## 1. Verify Current Version

    ng version

Ensure your project is using Angular 16.

------------------------------------------------------------------------

## 2. Update Global Angular CLI

    npm uninstall -g @angular/cli
    npm install -g @angular/cli@17

------------------------------------------------------------------------

## 3. Update Angular Core & CLI

Inside your project:

    ng update @angular/core@17 @angular/cli@17

This will:

-   Update Angular packages
-   Update dependencies
-   Adjust configurations

------------------------------------------------------------------------

## 4. Update Angular Material (If Used)

    ng update @angular/material@17

------------------------------------------------------------------------

## 5. Update Required Dependencies

Angular 17 requires:

    TypeScript ~5.2
    Node 18+

Then run:

    npm install

------------------------------------------------------------------------

## 6. Run the Application

    ng serve

Verify:

-   App compiles successfully
-   No runtime errors
-   Features working correctly

------------------------------------------------------------------------

## 7. (Optional) Migrate to New Control Flow Syntax

Old:

    <div *ngIf="isVisible">Content</div>

New:

    @if (isVisible) {
      <div>Content</div>
    }

------------------------------------------------------------------------

## 8. (Optional) Migrate \*ngFor

Old:

    <div *ngFor="let item of items">{{ item }}</div>

New:

    @for (item of items; track item) {
      <div>{{ item }}</div>
    }

------------------------------------------------------------------------

## 9. (Optional) Use Deferrable Views

    @defer {
      <heavy-component />
    }

------------------------------------------------------------------------

## Key Notes

-   Upgrade one version at a time
-   Always commit before upgrading
-   Use ng update instead of manual edits
-   Angular 17 introduces new template syntax
-   Existing code continues to work
-   New syntax adoption is optional

------------------------------------------------------------------------

## Summary

Angular 17 focuses on:

-   New control flow syntax
-   Better performance
-   Improved developer experience

Migration is smooth with minimal breaking changes.
