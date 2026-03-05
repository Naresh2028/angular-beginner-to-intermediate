# Observer

Moving to the second core pillar: **the Observer.** 

If the Observable is the radio station broadcasting music, the Observer is the Radio Receiver that listens and decides what to do with the sound.

## Definition

An Observer is simply a consumer of values delivered by an Observable. In code, it is just an object with three specific callback functions: next, error, and complete.

It defines three handlers/callback functions:

- **next()** → handles incoming data  
- **error()** → handles errors  
- **complete()** → executes when stream finishes  

---

## Analogy

Think of a **news subscriber**.

- News channel → Observable  
- Subscriber → Observer  
- Subscriber receives news updates.

The observer decides how to react to the information.

---

## Production‑Level Example

```ts
export class AppComponent implements OnInit, OnDestroy {
  subscription!: Subscription;

  ngOnInit(): void {
    const myObservable = new Observable();

    const myObserver = {
      // 1. Mandatory: Handles each new value
      next: (data: any) => {
        console.log('observer got a data :' + data);
      },

      // 2. Optional: Handles "something went wrong"
      error: (err: any) => {
        console.log('Observer got an error :' + err);
      },

      // 3. Optional: Handles "no more data coming"
      complete() {
        console.log('Observer Completed');
      },
    };

    myObservable.subscribe(myObserver);
  }

  ngOnDestroy(): void {
    if (this.subscription) {
      console.log('UnSubscribed Successfully');
      this.subscription.unsubscribe();
    }
  }
}
```

### Why the Observer is crucial for .NET Devs?

- In C#, when you use a foreach loop, the loop "handles" the data. If an exception occurs, the loop breaks. 

- In RxJS, the Observer replaces the loop body. The error callback replaces the catch block, and the complete callback replaces the code that runs after the loop finishes.

---

