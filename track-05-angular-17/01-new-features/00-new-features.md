# NEW FEATURES (Angular 17)

## List the Features

Angular 17 focuses on **simplifying templates, improving performance, and enhancing developer experience**.

Key features:

1. New Control Flow Syntax (@if, @for, @switch) (Developer preview)
2. Deferrable Views (@defer)
3. Hydration (Production Ready)
4. Standalone by Default (ng new my-app)
5. SSG (Improved build time), & SSR (Runtime performance)
6. esbuild and Vite-based Dev Server enabled by default
7. Continued Signals Integration
8. Custom Input Transforms 

---

# 1. NEW CONTROL FLOW

In Angular 17, the framework introduced a Built-in Control Flow that moves conditional logic and loops directly into the compiler. While it launched in "Developer Preview" in v17.

### Defination

Built-in Control Flow is a new, block-based syntax (@if, @for, @switch) used in Angular templates to handle dynamic rendering. Unlike the older structural directives (*ngIf, *ngFor) which are based on HTML attributes and require CommonModule imports.

It is "built-in," meaning it is available in every component automatically without any extra configuration or imports.


## 1. Conditional Rendering (*ngIf vs @if)

### Older Way (v16 and below):

You had to use a separate <ng-template> and a reference variable to handle the "else" case.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div *ngIf="isLoaded; else loading">
      <p>Data is here!</p>
    </div>

    <ng-template #loading>
      <p>Loading...</p>
    </ng-template>
  `,
})
export class ControlFlowComponent {
  isLoaded = false;

  constructor() {
    setTimeout(() => (this.isLoaded = true), 2000);
  }
}
````


### New way

No imports. Cleaner, more readable syntax that looks like standard JavaScript.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `

    @if (isLoaded) {
      <p>Data is here</p>
    } @else {
      <p>Loading...</p>
    }

  `,
})
export class ControlFlowComponent {
  isLoaded = false;

  constructor() {
    setTimeout(() => (this.isLoaded = true), 2000);
  }
}
````

**Reason To Change**

- The old way was visually noisy and required managing "template references" (#loading) which are hard to track in large files.

---

## 2. Looping (*ngFor vs @for)

Old Way:
Manual trackBy function in the .ts file was mandatory for performance but often ignored.


````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [NgForOf, NgIf],  // Import Required
  template: `
    <div *ngIf="Users.length > 0">
      <div *ngFor="let user of Users">
        <p>{{ user.userName }}</p>
      </div>
    </div>

    <div *ngIf="Users.length === 0">
      <p>User data is Empty</p>
    </div>
  `,
})
export class ControlFlowComponent {
  Users = [
    { id: 1, userName: 'Naresh', isWorking: true },
    { id: 2, userName: 'alien', isWorking: false },
    { id: 3, userName: 'Larthick', isWorking: true },
  ];

  constructor() {}
}
````

### New Way

The track property is now mandatory, and there is a built-in block for empty lists.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `

  <ul>
    @for (item of Users; track item.id) {
      <li>{{item.userName}}</li>
    } @empty {
      <li>No items to display...</li>
    }
  </ul>
  `,
})
export class ControlFlowComponent {
  Users = [
    { id: 1, userName: 'Naresh', isWorking: true },
    { id: 2, userName: 'alien', isWorking: false },
    { id: 3, userName: 'Larthick', isWorking: true },
  ];

  constructor() {}
}
````
 
**Reason to Change**

- By making track mandatory, Angular prevents common performance bugs where the entire list re-renders unnecessarily. 

- The @empty block also removes the need for a separate *ngIf="items.length === 0" check.


---

## 3. Switching: @switch vs *ngSwitch

### Older Way (v16 and below):

Required an attribute on a container and then specific directives on each child element.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [NgSwitch, NgSwitchCase, NgSwitchDefault],
  template: `
    <div [ngSwitch]="UserRole">
      <div *ngSwitchCase="'admin'">
        <p>Welcome Admin..</p>
      </div>
      <div *ngSwitchCase="'user'">
        <p>Welcome User..</p>
      </div>
      <div *ngSwitchDefault>
        <p>Welcome Guest...</p>
      </div>
    </div>
  `,
})
export class ControlFlowComponent {
  UserRole = ['admin', 'user'];
}
````

### New Way (v17+):

this is now a "pure" standalone component with zero external dependencies for its logic.

````ts
@Component({
  selector: 'app-control-flow',
  standalone: true,
  imports: [],
  template: `
    @switch (UserRole) {
      @case ('admin') {<p>Welcome Admin!</p>}
      @case ('user') {<p>Welcome User!</p>}
      @default {
        <p>Welcome Guest!</p>
      }
    }
  `,
})
export class ControlFlowComponent {
  UserRole: 'admin' | 'user' | 'guest' = 'guest';
}
````

**Reason to Change**

- The old *ngSwitch was technically a set of separate directives working together, which was slower to process. 

- The new @switch is a single block of logic, making it significantly faster for the browser to execute.

### Key Take Aways

- Performance: Up to 90% faster execution for complex loops and conditionals.

- Bundle Size: Because these are built into the compiler, they don't require the extra code associated with the NgIf and NgFor classes.


---

# 5. SSR (RUNTIME PERFORMANCE), & SSG (IMPROVED BUILD TIME)

Before knowing SSR, and SSG. We should be have aware of CSR terminology.

## 1. CSR (Client-Side Rendering)

Definition: In CSR, the server sends a "hollow" HTML shell. The browser downloads a large JavaScript bundle (the "Lego bricks") and then builds the entire UI on the user's computer.

## Example: The "Hollow" Response
When you visit a CSR site, if you "View Page Source," this is all you see:

```html
<html>
  <body>
    <app-root></app-root> 
    <script src="main.js"></script> </body>
</html>
````

The Process:

1. Browser: Requests index.html.

2. Server: Sends the empty shell above.

3. Browser: Downloads main.js.

4. Browser: Executes JS → Calls .NET API → Gets JSON → Builds the HTML tags inside <app-root>.

### The Reality of the CSR "Conversation"
 
Think of CSR like ordering a "Build-Your-Own" Pizza kit online:

Step 1 (The Box): You request the site. The server sends you the "Box" (index.html). It's empty.

Step 2 (The Manual): Inside the box is a note saying, "Go get the instructions." Your browser then makes a second request to download the main.js.

Step 3 (The Ingredients): Once the main.js is running, it realizes it needs data. It makes a third request to your .NET API to get the JSON "Ingredients."

----

## 2. SSR (Server-Side Rendering)

Definition: In SSR, the server (Node.js) runs your Angular code, fetches the data, and constructs the Full HTML before sending it to the browser. The user sees the content immediately.


## Example: The "Finished" Response

When you visit an SSR site, the "View Page Source" shows the actual content:

````html
<html>
  <body>
    <app-root>
      <nav>Home | Products</nav>
      <h1>iPhone 15 Pro</h1> <p>Price: $999</p>
    </app-root>
    <script src="main.js"></script>
  </body>
</html>
````

The Process:

1. Browser: Requests index.html.

2. Server (Node): Runs Angular → Calls .NET API → Fills HTML with data.

3. Server: Sends the completed HTML.

4. Browser: Displays HTML immediately → Downloads JS → Hydrates (connects buttons).

----

## 23 SSG (Server-Side Generation)

SSG is a technique where the Angular CLI runs your application during the ng build process to create a set of static .html files for every route you specify. When a user visits your site, the server simply hands over these pre-made files. 

- There is no "cooking" (rendering) involved at the moment of the request.

## Why it was Introduced (The Purpose)

1. **SSR Server Costs** : SSR requires a live Node.js server to "render" on every request. SSG creates files once, so you can host them on cheap "static" hosts (like GitHub Pages or Azure Static Web Apps) without Node.js.

2. **Security** : There is no server-side code running when the user visits, so there is no "attack surface" for hackers to exploit during the page request.

## Example ( How it works in Angular 17)

### 1. Enabling SSG

When you create a new project with ng new --ssr, Angular enables both SSR and SSG by default. You can control SSG specifically in your angular.json.

angular.json configuration:

```json
"prerender": {
  "routes": [
    "/",
    "/about",
    "/contact"
  ]
}
```

### 2. The Build Output

When you run ng build, look at your dist/browser folder. You will see something like this:

- index.html (Home page)

- about/index.html (Pre-rendered About page)

- contact/index.html (Pre-rendered Contact page)


### Rendering Approaches Comparison

| Feature              | CSR (Client-Side Rendering)         | SSR (Server-Side Rendering)         | SSG (Static Site Generation)         |
|----------------------|-------------------------------------|-------------------------------------|--------------------------------------|
| When is HTML built?  | In the Browser (Runtime).           | On the Server (Runtime).            | At Build Time (Deploy time).         |
| Best For             | Dashboards / Apps.                  | Real-time content (News/Prices).    | Blogs / Documentation / Marketing.   |
| SEO                  | ⚠️ Risky.                           | ✅ Excellent.                        | ✅ Excellent.                         |
| Server Requirement   | Any File Server.                    | Must have Node.js.                  | Any File Server.                      |




