
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

## 1️⃣ Service — Create API Calls (user.service.ts)

```ts

@Injectable({
  providedIn: 'root',
})
export class UserService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private url: string,
  ) {}

  getAllResponseData(): Observable<any> {
    const users$ = this.http.get(`${this.url}/users`);
    const post$ = this.http.get(`${this.url}/posts`);
    const comments$ = this.http.get(`${this.url}/comments`);

    return forkJoin({
      users: users$,
      post$: post$,
      comments$: comments$,
    });
  }
}

What happens here

All three APIs run at the same time, but forkJoin emits only after all are completed.

```

## 2️⃣ Component — Consume the Service (app.component.ts)

````ts
export class DashboardComponent implements OnInit {
  Users: User[] = [];
  Posts: Post[] = [];
  Comments: Comment[] = [];

  loading: boolean = true;

  constructor(private userService: UserService) {}

  ngOnInit(): void {}

  GetDashBoardData() {
    this.userService.getAllResponseData().subscribe({
      next: (data) => {
        if (data) {
          this.Comments = data.comments$;
          this.Posts = data.post$;
          this.Users = data.users;

          this.loading = false;
        }
      },
    });
  }
}

````

## 3️⃣ Template — Display Result (app.component.html)

````html

<h2>ForkJoin Dashboard Example</h2>

<button class="btn btn-primary" (click)="GetDashBoardData()">
Click
</button>

<div *ngIf="loading">
  Click to fetch all API data...
</div>

<div *ngIf="!loading">

  <h3>Users</h3>
  <ul>
    <li *ngFor="let user of Users">
      {{ user.name }}
    </li>
  </ul>

  <h3>Posts</h3>
  <p>Total Posts: {{ Posts.length }}</p>

  <h3>Comments</h3>
  <p>Total Comments: {{ Comments.length }}</p>

</div>
````

### 5️⃣ Browser Output

When the app loads:

````

ForkJoin Dashboard Example

Users
Leanne Graham
Ervin Howell
Clementine Bauch
...

Posts
Total Posts: 100

Comments
Total Comments: 500
````

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

