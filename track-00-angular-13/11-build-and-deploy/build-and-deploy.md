# 1. Build Optimization Concepts

Before we deploy, we should ensure our "dist" folder is as lean as possible.

- **AOT (Ahead-of-Time) Compilation:** Always use AOT for production. It converts your HTML and TypeScript into efficient JavaScript during the build process rather than in the browser. This catches template errors early and makes the app load faster.

- **Tree Shaking:** This is a build step that "shakes off" unused code from your third-party libraries. If you import a massive library like lodash but only use one function, tree shaking ensures the other 99% isn't shipped to your users.

- **Differential Loading:** Modern Angular builds create two sets of bundles: one for modern browsers (using ES2020+) and one for older browsers (ES5). The browser automatically chooses the smaller, faster version it supports.

# 2. Deployment Best Practices

## A. Environment Configuration

Never hardcode API URLs in our services. Use the src/environments/ folder.

- **Best Practice:** Create an environment.prod.ts for our production .NET API and an environment.ts for our local development.

- **Command:** ng build --configuration production ensures the production settings are swapped in automatically.

## B. Lazy Loading & Initial Bundle Size

The "Initial Bundle" is what the user must download before they see anything.

- **The Goal:** Keep the initial bundle under 500kb.

- The Fix: Use Lazy Loading for all features. If a user is on the "Login" page, they shouldn't be downloading the code for the "Dashboard" or "Admin Settings."

## C. Service Workers & PWA

- For high-performance apps, consider making your app a Progressive Web App (PWA).

- **Benefit:** This allows the browser to cache your scripts and CSS locally, so the second time a user visits, the app loads almost instantly, even on slow 3G networks.

---

# 3. Security in Deployment

- **Content Security Policy (CSP):** Configure our web server (Nginx/IIS) to send CSP headers. This prevents malicious scripts from running in your app (XSS protection).

- **Subresource Integrity (SRI):** Angular can automatically add hashes to your <script> tags. This ensures that if a hacker modifies your JavaScript files on the server, the browser will refuse to run them.

- **Disable Source Maps:** In production, ensure source maps are disabled ("sourceMap": false in angular.json). This prevents competitors or hackers from easily reading your original TypeScript source code.

# 4. A Note for .NET Engineers

When deploying your Angular app alongside a .NET Core API:

- **CORS:** Ensure your production API only allows requests from your specific production domain.

- **Static Files:** In .NET Core, use app.UseStaticFiles() and app.MapFallbackToFile("index.html") to ensure that when a user refreshes the page on an Angular route (like /dashboard), the server knows to serve the Angular app instead of a 404.

