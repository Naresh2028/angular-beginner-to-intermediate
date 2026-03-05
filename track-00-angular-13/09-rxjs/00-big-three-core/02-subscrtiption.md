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

## Production‑Level Example

```ts
const subscription = dataStream$.subscribe(observer);
```

Output:

```
Received: Data 1
Received: Data 2
Received: Data 3
Stream Completed
```

---

## Unsubscribing

```ts
subscription.unsubscribe();
```

Stops the Observable stream and prevents memory leaks.

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

