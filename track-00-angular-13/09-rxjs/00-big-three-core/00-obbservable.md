
# The Three Big Core Concepts in RxJS

1. Observable
2. Observer
3. Subscription

---

# Observable

## Definition

An **Observable** is a data stream that can emit multiple values over time.  
It represents asynchronous data sources such as:

- HTTP responses
- User events
- Timers
- WebSocket messages

Observables are **lazy**, meaning they start producing values only when subscribed.

---

## Analogy

Think of **a YouTube live stream**.

- The live stream → Observable  
- People watching → Observers  
- Clicking subscribe → Subscription  

The stream becomes meaningful only when someone watches it.

---

## Production‑Level Example

```ts
import { Observable } from 'rxjs';

const dataStream$ = new Observable(observer => {

  observer.next('Data 1');
  observer.next('Data 2');
  observer.next('Data 3');

  observer.complete();

});
```

---


