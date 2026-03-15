
# forkJoin

## Definition

`forkJoin` is an RxJS operator used to run **multiple observables in parallel** and wait until **all of them complete**.

Once every observable finishes, `forkJoin` emits a **single combined result** containing the **last emitted value from each observable**.

Key characteristics:

- Executes observables **in parallel**
- Waits for **all observables to complete**
- Emits **one final combined result**
- Similar to **Promise.all()** in JavaScript

---

## Analogy

Think of **ordering food for a group at a restaurant**.

- One person orders pizza  
- Another orders burgers  
- Another orders drinks  

The table starts eating **only when all orders arrive**.

`forkJoin` behaves the same way:

All requests run **simultaneously**, but the result arrives **only after everything is finished**.

---

## Scenario

### Scenario: Loading Dashboard Data

A dashboard may require data from multiple APIs:

- User profile
- Notifications
- Recent orders

Instead of waiting for each request one by one, we run them **in parallel** and display the page when **all data is ready**.

---

## Production Example (Angular)

```ts

```



---

## Another Example

```ts
forkJoin([
  this.http.get('/api/products'),
  this.http.get('/api/categories'),
  this.http.get('/api/reviews')
]).subscribe(([products, categories, reviews]) => {

  console.log(products);
  console.log(categories);
  console.log(reviews);

});
```

---

## Key Takeaways

- `forkJoin` runs **multiple observables in parallel**
- Emits **only once after all observables complete**
- Ideal for **loading multiple resources simultaneously**
- Similar concept to **Promise.all()**
- Commonly used for **dashboard or page initialization**

