# Observer

## Definition

An **Observer** listens to values emitted by an Observable.

It defines three handlers:

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
const observer = {
  next: (value: string) => console.log('Received:', value),
  error: (err: any) => console.error('Error:', err),
  complete: () => console.log('Stream Completed')
};
```

---

