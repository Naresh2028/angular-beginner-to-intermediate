
# APPLICATION FILES

<img width="446" height="343" alt="image" src="https://github.com/user-attachments/assets/cc2174e8-eaae-489c-b27f-d6883873c233" />

We go one by one based on the image.

## 1️⃣ src/ — What Is This Folder?

This is the application source code.

Everything inside src/ eventually becomes part of what the browser runs.

Everything outside src/ is tooling.

Simple rule:

- Outside src → build system

- Inside src → browser application

That mental boundary is important.

## 2️⃣ index.html

This is the first file the browser loads.

When you run:
- ng serve
The browser loads index.html.

Inside it, you will see:
- <app-root></app-root>

This tag is critical.

Angular replaces <app-root> with your entire application.

If you delete <app-root>, your app will compile but show nothing.

So:

- index.html = Browser entry point
- main.ts = Angular entry point

Don’t confuse them.

## 3️⃣ main.ts

This is where Angular starts.

Inside you’ll see something like:

-- platformBrowserDynamic().bootstrapModule(AppModule)

This line tells Angular:

“Start the application using AppModule.”

This file connects Angular to the browser.

If this file bootstraps the wrong module:
- The app fails.

## 4️⃣ app/ Folder

This is the heart of your application.

Everything you build goes here.

By default you’ll see:

````sql
app/
  app.component.ts
  app.component.html
  app.component.css
  app.module.ts
````

### app.module.ts

This is the root module.

It defines:

1. What components exist 
2. What modules are imported
3. Which component bootstraps first

It is the brain of your Angular app structure.

Angular always needs at least one module.

### app.component.*

This is the root component.

It controls what appears inside <app-root>.

So flow is:

- index.html → <app-root> → AppComponent → UI appears

## 5️⃣ assets/

This folder stores static files.

Examples:

1. Images 
2. Fonts
3. Icons

These files are not compiled.

They are copied directly into the build output.

If you put an image here:
You can access it like:
- assets/logo.png

Angular won’t transform it.

## 6️⃣ environments/

This folder usually contains:
- environment.ts
- environment.prod.ts

These files store configuration values.

Example:

````sql

export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000'
};

````
When you run:
- ng build --prod

Angular replaces environment.ts with environment.prod.ts automatically.

This allows:

1. Different API URLs

2. Feature flags

3. Production settings

Without environments:
You’d hardcode everything.

## 7️⃣ styles.css

Global stylesheet.

Applies to entire application.

Unlike component styles:
This affects everything.

## 8️⃣ polyfills.ts

This file loads polyfills.

Polyfills are compatibility patches for older browsers.

Example:
If a browser doesn’t support some modern feature, polyfills help simulate it.

In modern Angular versions this file is less visible, but in Angular 13 it exists.

## 9️⃣ test.ts

Entry point for unit testing.

Used by Karma test runner.

It initializes Angular testing environment.

Not used in production.

## Now Let’s Connect It Logically

When you run ng serve:

1. angular.json says main is src/main.ts

2. main.ts bootstraps AppModule

3. AppModule loads AppComponent

4. AppComponent renders inside <app-root>

5. index.html contains <app-root>

6. Browser shows UI

Everything else supports that flow.
