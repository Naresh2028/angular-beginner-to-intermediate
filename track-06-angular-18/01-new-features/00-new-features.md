
## 1.  Experimental Zoneless Change Detection

This allows you to remove zone.js from your project. Angular 18 introduced a new way for the framework to "hear" about changes without monkey-patching every browser event.

How: Use provideExperimentalZonelessChangeDetection() in your app config.

### Example :

````ts
import { ApplicationConfig, provideExperimentalZonelessChangeDetection, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';
import { provideClientHydration } from '@angular/platform-browser';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
     provideRouter(routes),
      provideClientHydration(),
      provideExperimentalZonelessChangeDetection()
    ]
};

````

Result: Better performance and smaller bundles for the application.

---

## 2. Unified Form Events

Angular 18 added a new .events property to all form controls (FormControl, FormGroup).

Why it's cool: Instead of subscribing to valueChanges and statusChanges separately, you get one stream of events like PristineChangeEvent, TouchedChangeEvent, and ValueChangeEvent.

---

## 3. Signal-Based Component APIs (Stable)

The way you handle data in and out of components has been "Signal-ified":

input(): Reactive inputs that you can use in computed() signals.

output(): A simpler, lighter way to emit events to parent components.

model(): Enables easy two-way data binding with signals.

effect(): An effect() is a block of code that registers itself as a consumer of any Signal used inside it.
