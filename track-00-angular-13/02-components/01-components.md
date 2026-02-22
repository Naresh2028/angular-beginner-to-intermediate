# 01 — Components (All-in-One)

## 1. Definition
A **Component** in Angular 13 is the smallest self-contained UI building block that combines a template (view) with behavior (class logic), and Angular uses it to render dynamic parts of the application.

---

## 2. Analogy
A component is like a **widget on a dashboard** — just as a speedometer or a fuel gauge is a reusable, independent part of a car dashboard, an Angular component is a reusable piece of the UI that can stand alone or be combined with others.

---

## 3. What Problem It Solves in Angular 13
Without components, apps would become monolithic HTML and JavaScript, making UI development hard to organize, reuse, and maintain; components enable modular architecture, encapsulated behavior, and scalable UI structure.

---

## 4. CLI Command
To generate a new component in Angular 13, run:

```bash
ng generate component components/player
```

This creates a Player component under the `components` folder.

---

## 5. Generated File Structure
After running the command above, you will see:

```
components/
  player/
    player.component.ts
    player.component.html
    player.component.css
    player.component.spec.ts
```

Explanation:

- `.ts` → component logic  
- `.html` → template view  
- `.css` → component styles  
- `.spec.ts` → unit test file

---

## 6. Minimal Working Example

### Step A — Component Code

`player.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-player',
  templateUrl: './player.component.html',
  styleUrls: ['./player.component.css']
})
export class PlayerComponent {
  name = 'Player One';
}
```

### Step B — Template

`player.component.html`

```html
<h3>{{ name }} Ready!</h3>
<button>Start Game</button>
```

### Step C — Use It in App

In `app.component.html`, add:

```html
<app-player></app-player>
```

Run:

```bash
ng serve
```
<img width="434" height="344" alt="image" src="https://github.com/user-attachments/assets/22efc60e-bf45-4fa2-b9b6-a6241d9db0b3" />

---

## 7. What Happens If Misconfigured

### Case A — Component Not Declared
If `PlayerComponent` is not declared in any module (e.g., AppModule or a feature module), Angular will throw:

```
Error: Component PlayerComponent is not part of any NgModule
```

### Case B — Wrong Selector
If you use the wrong selector in `app.component.html`, Angular will display:

```
'app-wrong-selector' is not a known element
```

These errors prove that Angular only recognizes components declared in modules and rendered with correct selectors.

<img width="1736" height="870" alt="image" src="https://github.com/user-attachments/assets/cee2a5c7-9108-466b-98ab-0f3313c146a7" />


---

## 8. Expected Output

When everything is configured correctly:

- The application runs with `ng serve`
- Browser displays:
  ```
  Player One Ready!
  [Start Game Button]
  ```
- Component renders exactly where `<app-player></app-player>` is placed
- No errors in console or compilation

---

## Summary
Angular components are the heart of UI structure. They solve modularization problems by combining view and logic, and they must be declared in modules and used with correct selectors to work.

