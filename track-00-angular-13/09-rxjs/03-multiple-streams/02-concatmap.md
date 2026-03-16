# CONCAT



## Definition

concatMap() is a Transformation Operator that projects each source value into an internal Observable, but it maintains a strict order. It serializes the stream.

If 5 values come in quickly, concatMap will subscribe to the first one, buffer (hold) the other 4, and only start the second one once the first has officially completed. It’s like a single-lane road where cars must follow each other one by one.


It is commonly used when **order of operations matters**.

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
import { HttpClient } from '@angular/common/http';
import { Inject, Injectable } from '@angular/core';
import { Observable, concat, concatMap, from } from 'rxjs';
import { API_URL } from 'src/app/Class/token';
import { User } from 'src/app/Interfaces/user';

@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private url: string,
  ) {}

  getSequentialOrder(): Observable<User> {
    const id = [1, 2, 3, 4, 5];

    return from(id).pipe(
      concatMap((id) => this.http.get<User>(`${this.url}/${id}`)),
    );
  }
}

```

### order.component.ts Component 

````ts
export class OrderComponent implements OnInit {
  Users: User[] = [];

  constructor(private userService: UserService) {}

  ngOnInit(): void {}

  fetchRecord() {
    this.Users = [];

    this.userService.getSequentialOrder().subscribe({
      next: (data) => {
        if (data) {
          console.log('Got the Api Reponse :' + data);
          this.Users.push(data);
        }
      },
    });
  }
}

````

### Order.component.html (Template)

````html
<button (click)="fetchRecord()" class="btn btn-primary">Fetch User
    Record</button>

<div class="card-header">
    <ul class="card-body">
        <li *ngFor="let user of Users">
            {{user.id}}
            {{user.name}}
        </li>
    </ul>
</div>
````


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

## Why concatMap Is Used Here

Use concatMap when:

- Order must be preserved

- API calls depend on sequence

- Avoid overwhelming backend

- Need predictable request flow
