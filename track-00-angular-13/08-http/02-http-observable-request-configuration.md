# Observable APIs Request Configuration

## Definition

Angular HttpClient allows configuring HTTP requests using options such
as:

-   URL Parameters (Query Params)
-   Request Headers
-   Response Types

All HTTP methods return Observables, allowing reactive handling of
responses.

Configuration ensures flexible and production-ready API communication.

------------------------------------------------------------------------

# 1️⃣ URL Parameters (Query Parameters)

## Definition

Query parameters are key-value pairs appended to a URL.

Used for:

-   Filtering
-   Pagination
-   Searching
-   Sorting

------------------------------------------------------------------------

## Production-Level Example

## Service File (API Request)

``` ts
import { Injectable, Inject } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Post } from 'src/app/Interfaces/post';
import { API_URL } from 'src/app/Class/token';

@Injectable({
  providedIn: 'root',
})
export class PostService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private api: string,
  ) {}

  getLimitedUserList(userId: number, limit: number): Observable<Post[]> {
    const params = new HttpParams()
      .set('userId', userId.toString())
      .set('_limit', limit.toString());

    return this.http.get<Post[]>(this.api, { params });
  }
}

```
### Service File: The "Communicator"

- **Immutability of HttpParams:** Remember that HttpParams is immutable. 

- Each **.set()** returns a new instance. Chaining them as you did (.set().set()) is the correct way to build a query string like ?userId=1&_limit=5.

## Component Implementation

````ts
@Component({
  selector: 'app-search-post',
  templateUrl: './search-post.component.html',
  styleUrls: ['./search-post.component.css'],
})
export class SearchPostComponent implements OnInit {
  UserId: number = 1;
  limit: number = 5;
  results: Post[] = [];

  constructor(private postService: PostService) {}

  ngOnInit(): void {}

  Search() {
    this.postService.getLimitedUserList(this.UserId, this.limit).subscribe({
      next: (data) => {
        if (data) {
          this.results = data;
        }
      },
    });
  }
}
````

### Component Implementation: The "Coordinator"

- **Defensive Initialization:** By setting UserId: number = 1; and limit: number = 5, We prevent "undefined" errors if the user clicks "Submit" immediately after the page loads.

## Template View

````html

<div class="container">
    <label for="UserId" class="form-label">UserId : </label>
    <input class="form-control" type="number" name="UserId"
        id="UserId"
        [(ngModel)]="UserId" />

    <label for="limit" class="form-label">Limit : </label>
    <input class="form-control" type="number" name="limit"
        id="limit"
        [(ngModel)]="limit" />
    <button class="btn btn-primary" (click)="Search()"> Sumbit </button>
</div>

<div class="card mt-3 shadow-sm" *ngIf="results.length > 0">
    <div class="card-header bg-primary text-white">
        <h5 class="mb-0">Search Results</h5>
    </div>

    <ul class="list-group list-group-flush">
        <li class="list-group-item" *ngFor="let result of results">
            <h6 class="fw-bold text-uppercase mb-1">Title: {{ result.title
                }}</h6>
            <p class="mb-0 text-muted">Body: {{ result.body }}</p>
        </li>
    </ul>

    <div class="card-footer text-end">
        <small class="text-secondary">Total Results: {{ results.length
            }}</small>
    </div>
</div>

<div class="card" *ngIf="results.length == 0">
    <p>Empty results found! plese search for different results...</p>
</div>
````

### Output

<img width="704" height="885" alt="image" src="https://github.com/user-attachments/assets/61228460-d4a9-44bf-95fb-96d4f89a51f6" />


------------------------------------------------------------------------

# 2️⃣ Request Headers

## Definition

Headers send additional metadata with requests.

Common uses:

-   Authorization tokens
-   Content-Type
-   Custom client identifiers

------------------------------------------------------------------------

## Production-Level Example

``` ts
import { HttpHeaders } from '@angular/common/http';

getSecureData() {

  const headers = new HttpHeaders({
    'Authorization': 'Bearer my-token',
    'Content-Type': 'application/json'
  });

  return this.http.get<any>(
    'https://api.example.com/secure-data',
    { headers }
  );
}
```

------------------------------------------------------------------------

## Dynamic Header Example

``` ts
const token = localStorage.getItem('authToken');

const headers = new HttpHeaders()
  .set('Authorization', `Bearer ${token}`);
```

------------------------------------------------------------------------

# 3️⃣ Response Types

## Definition

HttpClient allows configuring expected response format.

Default response type is JSON.

Available types:

-   json (default)
-   text
-   blob
-   arraybuffer

------------------------------------------------------------------------

## Production-Level Example

### Text Response

``` ts
this.http.get(
  'https://api.example.com/status',
  { responseType: 'text' }
);
```

------------------------------------------------------------------------

### File Download (Blob)

``` ts
this.http.get(
  'https://api.example.com/report',
  { responseType: 'blob' }
);
```

Used for:

-   PDF downloads
-   Image downloads
-   Excel export

------------------------------------------------------------------------

# Combined Production Example

``` ts
getFilteredUsers(page: number, search: string) {

  const params = new HttpParams()
    .set('page', page)
    .set('search', search);

  const headers = new HttpHeaders()
    .set('Authorization', 'Bearer token');

  return this.http.get<User[]>(
    'https://api.example.com/users',
    {
      params,
      headers,
      responseType: 'json'
    }
  );
}
```

------------------------------------------------------------------------

# Advanced: observe Full Response

``` ts
this.http.get<User[]>(
  'https://api.example.com/users',
  { observe: 'response' }
).subscribe(response => {
  console.log(response.status);
  console.log(response.headers);
  console.log(response.body);
});
```

Useful when:

-   Reading pagination headers
-   Checking status codes
-   Accessing metadata

------------------------------------------------------------------------

# Best Practices

-   Always use HttpParams for query parameters.
-   Never manually concatenate query strings.
-   Use interceptors for global headers (Auth).
-   Strongly type responses with generics.
-   Handle errors using RxJS operators.

------------------------------------------------------------------------

## Key Takeaways

-   Request configuration makes APIs flexible.
-   Query params control filtering and pagination.
-   Headers manage security and metadata.
-   Response type controls output format.
-   Essential for production-grade Angular applications.
