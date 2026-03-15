
# concatMap

## Definition

`concatMap` is an RxJS **higher-order mapping operator** that maps each emitted value from a source observable into an inner observable and **executes them sequentially**.

Key characteristics:

- Processes observables **one at a time**
- Waits for the **previous observable to complete**
- Maintains the **original order of emissions**
- Prevents concurrent execution

It is commonly used when **order of operations matters**.

---

## Scenario to Use (with Analogy)

### Scenario

Imagine an e‑commerce checkout workflow:

1. Create Order
2. Add Order Items
3. Process Payment
4. Send Confirmation Email

Each step must happen **in sequence**. If payment happens before the order is created, the workflow fails.

Using `concatMap` ensures that each step finishes **before the next begins**.

---

### Analogy

Think of a **bank queue**.

- Customers stand in line.
- The cashier handles **one customer at a time**.
- The next customer waits until the previous one finishes.

`concatMap` behaves exactly like this queue system.

---

## Example

### Sequential API Requests

```ts
import { from } from 'rxjs';
import { concatMap } from 'rxjs/operators';

const userIds = [1, 2, 3];

from(userIds)
  .pipe(
    concatMap(id => fetch(`https://api.example.com/users/${id}`))
  )
  .subscribe(response => {
    console.log(response);
  });
```

Execution order:

```
Request user 1 → completes
Request user 2 → starts
Request user 3 → starts
```

Requests run **one after another**, never in parallel.

---

### Production Example (Angular HTTP)

Uploading files sequentially:

```ts
from(files)
  .pipe(
    concatMap(file => this.http.post('/upload', file))
  )
  .subscribe(result => {
    console.log("Uploaded:", result);
  });
```

Each file upload waits until the **previous upload finishes**.

---

## Key Takeaways

- `concatMap` ensures **sequential execution**
- Maintains **original emission order**
- Prevents concurrent API calls
- Useful for **transaction workflows**
- Ideal for **sequential HTTP requests**
