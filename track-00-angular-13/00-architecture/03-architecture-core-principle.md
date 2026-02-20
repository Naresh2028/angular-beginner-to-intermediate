# 1️⃣ Angular vs Angular CLI

Angular is the framework.

It provides:

- Components
- Templates
- Dependency Injection
- Change Detection
- Routing
- Modules (NgModule in v13)

Angular CLI is the tool.

It provides:

- Project scaffolding (ng new)
- Development server (ng serve)
- Build system
- Testing setup
- Configuration management (angular.json)

Key clarity:

1. Angular can run without Angular CLI. 
2. Angular CLI cannot exist without Angular.

CLI is tooling. Angular is runtime framework.

If angular.json is deleted, Angular framework still works — but CLI cannot build or serve the app.

# 2️⃣ Build Time vs Runtime Separation

Build Time happens before the browser sees your app.

Build Time includes:

- TypeScript compilation
- Bundling with Webpack
- Reading angular.json
- Reading tsconfig
- Optimizing assets
- Replacing environment files

Runtime happens inside the browser.

Runtime includes:

- Loading index.html
- Executing JavaScript bundle
- Running main.ts
- Bootstrapping AppModule
- Rendering components
- Handling user interactions

Why this separation matters:

1. If something breaks before the browser loads → build problem.
2. If something breaks after UI loads → runtime problem.

This distinction makes debugging logical.

# 3️⃣ Role of AppModule (Angular 13 Context)

In Angular 13, AppModule is mandatory.

Angular does not bootstrap a component directly.

main.ts contains:

- bootstrapModule(AppModule)

AppModule defines:

- Which components exist
- Which modules are imported
- Which component starts first (bootstrap array)
- AppModule is the root container of the Angular application.

Without AppModule:
- Angular 13 cannot start.

1. Later Angular versions introduced standalone components.
2. But in Angular 13, everything revolves around NgModule.
