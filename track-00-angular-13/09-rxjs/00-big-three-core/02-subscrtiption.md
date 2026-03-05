# Subscription

## Definition

A **Subscription** represents the execution of an Observable.

When a subscription happens:

- The Observable starts producing data.
- The Observer starts receiving it.

It also allows cancelling the stream.

---

## Analogy

Think of **a Netflix subscription**.

- Without subscription → no streaming.
- With subscription → content streams.
- Cancel subscription → streaming stops.

---

## Example

```ts
    // 1. Start the execution
    const mySubscription = myObservable.subscribe(val => console.log(val));

    // 2. Stop the execution
    mySubscription.unsubscribe();
```
Its primary purpose is simple but vital: to stop the execution.

---

### Why is unsubscribe() important?

If your component fetches a list of posts every 5 seconds using interval(), and you navigate to a different page without unsubscribing, the browser will keep fetching those posts in the background. This leads to Memory Leaks and slow apps.

---

# Angular Production Example

Using HTTP Observable in Angular.

```ts
this.http.get<User[]>('api/users')
  .subscribe({
    next: users => console.log(users),
    error: err => console.error(err),
    complete: () => console.log('Request completed')
  });
```

---

# Key Takeaways

- **Observable** → Data source
- **Observer** → Handles emitted values
- **Subscription** → Starts and controls execution
- Observables support asynchronous streams
- Always unsubscribe for long‑running streams

