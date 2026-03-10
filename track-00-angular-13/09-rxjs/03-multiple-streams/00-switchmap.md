# SWITCHMAP

The switchMap() operator is often called the "Safe Search" or "Switching" operator. In the world of RxJS, it is the most powerful tool for handling Asynchronous Requests where only the latest result matters.

## 💡 What is switchMap()?

At its core, switchMap does two things simultaneously:

- Unsubscribes from the previous internal Observable (cancels the old task).

- Subscribes to a new internal Observable (starts the new task).

Imagine you are at a fast-food counter. You order a Burger. While the chef is cooking, you change your mind and shout, "Wait! Cancel the burger, I want a Pizza instead!" * The chef stops cooking the burger (Unsubscribe).

- The chef starts making the pizza (New Subscribe).

- You only receive the pizza.

### The Scenario: Product Category Switcher

### Scenario

In this scenario, if a user clicks "Electronics," then immediately clicks "Clothing" before the first request finishes, switchMap kills the "Electronics" request. This ensures the user never sees "Laptops" inside the "Clothing" section due to a slow network.

Imagine an e-commerce admin panel where clicking a category fetches a list of products.

## 1. The Service (product.service.ts)

```ts
export class PostService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private api: string,
  ) {}

  getProductsByCategory(category: string): Observable<any> {
    const url = `https://fakestoreapi.com/products/category/${category}`;
    return this.http.get<any[]>(url).pipe(delay(1000));
  }
}
````

- We add a manual delay to simulate a slow network so you can see switchMap in action!

## 2. The Component (dashboard.component.ts)

```ts
export class DashboardComponent implements OnInit {
  private categoryAction = new Subject<string>();

  isActive: boolean = false;

// The switchMap magic happens here
  posts$ = this.categoryAction.pipe(
    tap(() => (this.isActive = true)), // Start loader
    switchMap((category) => this.postService.getProductsByCategory(category)),
    tap(() => (this.isActive = false)), // Stop loader
  );

  constructor(private postService: PostService) {}

  ngOnInit(): void {}

  ChangeCategory(cat: string) {
    this.categoryAction.next(cat);
  }
}
````

##  3. The Template (dashboard.component.html)

```html
<div class="container mt-4">
  <div class="btn-group mb-3">
    <button (click)="ChangeCategory('electronics')" class="btn btn-outline-primary">Electronics</button>
    <button (click)="ChangeCategory('jewelery')" class="btn btn-outline-primary">Jewelery</button>
    <button (click)="ChangeCategory('mens-clothing')" class="btn btn-outline-primary">Men's Clothing</button>
  </div>

  <div *ngIf="isActive" class="text-info">🔄 Fetching fresh data (Previous request cancelled)...</div>

  <div *ngIf="posts$ | async as products">
    <div class="row">
      <div *ngFor="let p of products" class="col-md-4 mb-3">
        <div class="card h-100 p-2">
          <img [src]="p.image" style="height: 100px; object-fit: contain;">
          <p class="small fw-bold">{{ p.title }}</p>
          <span class="badge bg-success">${{ p.price }}</span>
        </div>
      </div>
    </div>
  </div>
</div>
````

### Output

<img width="1920" height="882" alt="image" src="https://github.com/user-attachments/assets/42d4d228-241d-48b4-8912-86893ebed0ad" />


### switchMap provides three major benefits:

- Automatic Cancellation: It kills the previous HTTP request mid-flight.

- Race Condition Prevention: It ensures that "Old Data" never overwrites "New Data."

- Memory Efficiency: It cleans up after itself by closing old streams.
