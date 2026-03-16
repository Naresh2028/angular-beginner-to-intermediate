# Migration Notes (Angular 14 → Angular 15)

## How can I transform my Angular 14 project into Angular 15?

Follow these steps to upgrade your Angular 14 project safely to Angular
15.

------------------------------------------------------------------------

## 1. Verify Current Angular Version

Run:

    ng version

Confirm the project is currently using **Angular 14**.

------------------------------------------------------------------------

## 2. Update Global Angular CLI

    npm uninstall -g @angular/cli
    npm install -g @angular/cli@15

------------------------------------------------------------------------

## 3. Update Angular Core and CLI

Inside the project directory:

    ng update @angular/core@15 @angular/cli@15

Angular CLI will automatically:

-   update Angular packages
-   update peer dependencies
-   adjust configuration files

------------------------------------------------------------------------

## 4. Update Angular Material (If Used)

    ng update @angular/material@15

------------------------------------------------------------------------

## 5. Update Dependencies

Angular 15 commonly requires:

    TypeScript ~4.8
    RxJS ~7.5
    Node 14.20+ / 16+

Then run:

    npm install

------------------------------------------------------------------------

## 6. Test the Application

Start the app:

    ng serve

Verify:

-   Application compiles successfully
-   No console errors
-   Core features work properly

------------------------------------------------------------------------

## 7. Optional: Adopt Standalone Components

Angular 15 stabilizes **Standalone Components**.

Example:

    @Component({
      selector: 'app-demo',
      standalone: true,
      template: `<h1>Standalone Component</h1>`
    })
    export class DemoComponent {}

Benefits:

-   Reduces NgModule complexity
-   Simplifies Angular architecture
-   Enables easier lazy loading

------------------------------------------------------------------------

## 8. Optional: Standalone Router APIs

Angular 15 supports standalone router configuration.

Example:

    bootstrapApplication(AppComponent, {
      providers: [
        provideRouter(routes)
      ]
    });

This removes the need for a traditional `AppModule`.

------------------------------------------------------------------------

# Key Notes

-   Upgrade Angular **one major version at a time**
-   Always commit code before running `ng update`
-   Prefer `ng update` instead of manually editing package.json
-   Run tests after upgrading
-   Gradually adopt standalone APIs if desired

------------------------------------------------------------------------

# Summary

Migration from Angular 14 to Angular 15 is typically smooth.

Major improvements after upgrading:

-   Stable Standalone Components
-   Improved Router APIs
-   Simplified architecture
-   Updated CLI and tooling

