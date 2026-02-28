# Service Definition & Metadata

## Definition

A Service in Angular is a class that contains reusable business logic.

Metadata is defined using the `@Injectable()` decorator, which tells
Angular:

-   How the service should be provided
-   Where it should be available
-   How it participates in the DI system

------------------------------------------------------------------------

## Analogy

Think of a shared utility room in an office.

-   Multiple employees use it.
-   It centralizes common tools.
-   It avoids duplication across departments.

Services centralize logic for reuse across components.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: API Data Service

``` ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Post } from 'src/app/Interface/post';

@Injectable({
  providedIn: 'root',
})
export class PostService {
  private API_URL = 'https://jsonplaceholder.typicode.com/posts';

  constructor(private http: HttpClient) {}

  getPost(): Observable<Post[]> {
    return this.http.get<Post[]>(this.API_URL);
  }
}

```

### Entity Class

```ts
export interface Post {
    userId:number,
    id:number,
    title:string,
    body:string
}
````

### Using Service in Component

``` ts
@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <h1 appHighlight class="card-title mb-3 text-primary">Angular Learning</h1>

    <table class="table table-striped table-bordered table-hover">
      <thead class="table-dark">
        <tr>
          <th>Number</th>
          <th>Title</th>
          <th>Body</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let item of postData; index as i">
          <ng-container *ngIf="i < 10">
            <td>{{ item.id }}</td>
            <td>{{ item.title }}</td>
            <td>{{ item.body }}</td>
          </ng-container>
        </tr>
      </tbody>
    </table>
  `,
})
export class AppComponent implements OnInit {
  postData: Post[] = [];

  constructor(private postService: PostService) {}

  ngOnInit(): void {
    this.postService.getPost().subscribe((data) => {
      this.postData = data;
    });
  }
}
```
## Output
<img width="1920" height="723" alt="image" src="https://github.com/user-attachments/assets/6c3df146-b465-4d36-9a50-e8dfe01a02f0" />

------------------------------------------------------------------------

## Key Takeaways

-   DI promotes loose coupling.
-   Injector resolves dependencies automatically.
-   `providedIn: 'root'` enables tree-shaking.
-   Services centralize reusable logic.
-   Metadata controls service lifecycle and scope.


