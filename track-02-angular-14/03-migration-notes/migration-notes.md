# Migration Notes (Angular 13 → Angular 14)

## How can I transform my Angular 13 project into Angular 14?

Follow these steps carefully to upgrade your Angular 13 project to
Angular 14.

------------------------------------------------------------------------

## 1. Check Current Angular Version

Run:

    ng version

Confirm the project is currently using **Angular 13**.

------------------------------------------------------------------------

## 2. Update Global Angular CLI

Update the global CLI first.

    npm uninstall -g @angular/cli
    npm install -g @angular/cli@14

------------------------------------------------------------------------

## 3. Update Project Dependencies

Inside your Angular project directory run:

    ng update @angular/core@14 @angular/cli@14

Angular CLI will:

-   update Angular packages
-   update TypeScript version
-   adjust configuration files

------------------------------------------------------------------------

## 4. Update Angular Material (If Used)

If your project uses Angular Material:

    ng update @angular/material@14

------------------------------------------------------------------------

## 5. Install Compatible TypeScript Version

Angular 14 requires **TypeScript 4.7+**.

Check in package.json:

    "typescript": "~4.7.x"

If needed:

    npm install typescript@4.7 --save-dev

------------------------------------------------------------------------

## 6. Run the Application

After updating dependencies run:

    npm install
    ng serve

Verify the application builds successfully.

------------------------------------------------------------------------

## 7. Optional Improvements After Migration

Angular 14 introduced **Typed Reactive Forms**.

You may gradually migrate forms.

Example:

Before:

    new FormControl('')

After:

    new FormControl<string>('')

------------------------------------------------------------------------

## 8. Optional: Try Standalone Components

Angular 14 introduces standalone components (developer preview).

Example:

    @Component({
      selector: 'app-demo',
      standalone: true,
      template: `<h1>Hello</h1>`
    })
    export class DemoComponent {}

This allows Angular apps without NgModules in future versions.

---

### Verify the Upgration (pack.json)

````json
{
  "name": "angular-13",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "watch": "ng build --watch --configuration development",
    "test": "ng test"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "^14.3.0",
    "@angular/common": "^14.3.0",
    "@angular/compiler": "^14.3.0",
    "@angular/core": "^14.3.0",
    "@angular/forms": "^14.3.0",
    "@angular/platform-browser": "^14.3.0",
    "@angular/platform-browser-dynamic": "^14.3.0",
    "@angular/router": "^14.3.0",
    "bootstrap": "^5.3.8",
    "rxjs": "~7.5.0",
    "tslib": "^2.3.0",
    "zone.js": "~0.11.4"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "^14.2.13",
    "@angular/cli": "^14.2.13",
    "@angular/compiler-cli": "^14.3.0",
    "@types/jasmine": "~3.10.0",
    "@types/node": "^12.11.1",
    "jasmine-core": "~4.0.0",
    "karma": "~6.3.0",
    "karma-chrome-launcher": "~3.1.0",
    "karma-coverage": "~2.1.0",
    "karma-jasmine": "~4.0.0",
    "karma-jasmine-html-reporter": "~1.7.0",
    "typescript": "~4.6.2"
  }
}
````

------------------------------------------------------------------------

# Key Notes

-   Always upgrade Angular **one major version at a time**
-   Backup your project before migration
-   Run `ng update` instead of manual package changes
-   Check Angular release notes for breaking changes
-   Test the application thoroughly after upgrading
-   Gradually adopt new features like **Typed Forms**

------------------------------------------------------------------------

# Summary

Migration from Angular 13 to Angular 14 is usually smooth because
Angular maintains strong backward compatibility.

Main improvements after upgrading:

-   Typed Reactive Forms
-   Better template diagnostics
-   Standalone components (preview)
-   Updated CLI tooling

