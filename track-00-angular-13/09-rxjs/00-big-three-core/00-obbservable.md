
# The Three Big Core Concepts in RxJS

1. Observable
2. Observer
3. Subscription

---

# Observable

## Definition

An Observable is a blueprint for a stream of data. It doesn't start "streaming" until someone subscribes to it.

It can emit three types of notifications:

- Next: A new value arrives (e.g., a bit of data from an API).

- Error: Something went wrong (the stream stops).

- Complete: No more data is coming (the stream stops).

---

## Analogy

Think of **a YouTube live stream**.

- The live stream → Observable  
- People watching → Observers  
- Clicking subscribe → Subscription  

The stream becomes meaningful only when someone watches it.

---

## The Basic Syntax

To create a custom Observable, we use the new Observable constructor. It takes a "subscriber" function that defines when to push data.

```ts
export class AppComponent implements OnInit, OnDestroy {
  subscription!: Subscription;

  ngOnInit(): void {
    const myObservable = new Observable((observer) => {
      observer.next('Hello Naresh');
      observer.next(30);

      setInterval(() => {
        observer.next('Stream is about to Complete');
        observer.complete();
      }, 2000);
    });

    this.subscription = myObservable.subscribe({
      next: (data) => console.log('Recieved :', data),
      complete() {
        console.log('Stream Completed');
      },
      error: (err) => console.log('Error happened'),
    });
  }

  ngOnDestroy(): void {
    if (this.subscription) {
      console.log('UnSubscribed Successfully');
      this.subscription.unsubscribe();
    }
  }
}
```

### Output

<img width="1374" height="797" alt="image" src="https://github.com/user-attachments/assets/d60b54fd-5d31-4e0a-b433-32f2ea39ec47" />


## Creating Observables from "Existing Things"

In 90% of your Angular work, you won't use new Observable(). You will use Creation Operators to turn arrays, events, or values into streams.

### A. From a Value: of()

Useful for mocking data or returning a single item as a stream.

```ts
export class AppComponent implements OnInit, OnDestroy {
  subscription!: Subscription;

  ngOnInit(): void {
    const ofValue = of({ id: '1', userName: 'Naresh' });

    this.subscription = ofValue.subscribe({
      next: (data) =>
        console.log(`User Details: ID:${data.id}, Name:${data.userName}`),
    });
  }

  ngOnDestroy(): void {
    if (this.subscription) {
      console.log('UnSubscribed Successfully');
      this.subscription.unsubscribe();
    }
  }
}
```

### Output
<img width="868" height="487" alt="image" src="https://github.com/user-attachments/assets/9e0a4268-1614-464f-ac31-cafd4926bf99" />

### B. From an Array: from()

Turns an array into a stream where each item is emitted one by one.

```ts
export class AppComponent implements OnInit, OnDestroy {
  subscription!: Subscription;

  ngOnInit(): void {
    const fromValue = from([1,3,5,8]);

    this.subscription = fromValue.subscribe({
      next:(data) => {
        console.log(`Numbers : ${data}`);
      }
    })
  }

  ngOnDestroy(): void {
    if (this.subscription) {
      console.log('UnSubscribed Successfully');
      this.subscription.unsubscribe();
    }
  }
}
```

### Output

<img width="1163" height="386" alt="image" src="https://github.com/user-attachments/assets/48d2e135-02d5-4115-baea-ebc36e60cc2f" />


---

### C. From a DOM Event: fromEvent()

Turns clicks or keystrokes into a continuous stream.

```ts

```

## Why is this better than a Promise?

As a .NET/Angular dev, you might ask: "Why not just use async/await and Promises?"

### Promise vs Observable in Angular

| Feature      | Promise                                | Observable                                      |
|--------------|----------------------------------------|------------------------------------------------|
| **Values**   | Emits only **one** value.              | Emits **multiple values** over time.           |
| **Lazy**     | Starts immediately when created.       | Starts only when someone **subscribes**.       |
| **Cancellable** | Cannot be cancelled easily.         | Can be cancelled by calling **unsubscribe()**. |
| **Operators** | No built-in tools like map or filter. | Has 100+ operators to transform data.          |


---

