# Modules (Hands-On Example)

Objective

Prove practically:

1. How to create a feature module

2. How to create a component inside that module

3. How Angular registers components

4. What error happens if a component is not registered

### Step 1 — Create a Feature Module

Run:

````sql

ng generate module features/game

````

Open game.module.ts.

You should see:

<img width="987" height="632" alt="image" src="https://github.com/user-attachments/assets/fe335d61-2162-4183-bcb1-a88fffb0ca3e" />


Observe carefully:

- It is NOT bootstrapped.

- It does NOT contain BrowserModule.

- It imports CommonModule.

- It has an empty declarations array.

This is a Feature Module.

It groups related functionality.

---

### Step 2 — Create a Component Inside That Module

Now run:

````sql

ng generate component features/game/score-board --module features/game

````

Important:

The --module flag tells Angular to auto-register this component inside GameModule.

---

### Step 3 — Observe How Angular Registers the Component

Open game.module.ts.

You should now see:

<img width="1256" height="559" alt="image" src="https://github.com/user-attachments/assets/f3408372-13b0-47b2-9df2-14bcc8bb3861" />

This is the critical moment.

Angular registers the component by adding it to:

declarations: []

This is how Angular knows:

“This component belongs to GameModule.”

Without this, Angular will not compile it.

- Folders do not matter.
- Registration matters.

---

### Step 4 — Import Feature Module into AppModule

Add import into AppModule:

````sql

  imports: [
    BrowserModule,
    AppRoutingModule,
    GameModule  
  ]

````

Now GameModule becomes part of the root application.

Without this import:
- GameModule is isolated and unused.

---

### Step 5 — Use the Component

Add into app.component.html:

- <app-score-board></app-score-board>

Update game.module.ts:

````sql
  exports:[
    ScoreBoardComponent
  ]
````

Expected output:

The ScoreBoardComponent renders on screen.

<img width="409" height="252" alt="image" src="https://github.com/user-attachments/assets/d234b202-ef84-46fe-a3c4-328af450bfad" />

---

### Step 6 — Misconfiguration Test

Remove:

- ScoreBoardComponent

from declarations & Save.

Angular should throw error similar to:

<img width="1091" height="424" alt="image" src="https://github.com/user-attachments/assets/bc51742b-322e-4c6d-95f7-66e5818f4827" />



This proves:

1. Angular does NOT discover components automatically.

2. It only compiles what is declared inside a module.

Put it back.

App works again.


What You Learned

- [ ] Feature modules group related components.

- [ ] Components must be declared.

- [ ] Feature modules must be imported into AppModule.

- [ ] Angular relies on NgModule metadata, not folders.

- [ ] Misconfiguration produces compile errors.

