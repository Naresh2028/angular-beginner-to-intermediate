# Core Functionality: Performing HTTP Requests

## Definition

Angular performs HTTP requests using the HttpClient service from
`@angular/common/http`.

HttpClient allows communication with backend APIs to:

-   Fetch data (GET)
-   Create data (POST)
-   Replace data (PUT)
-   Partially update data (PATCH)
-   Delete data (DELETE)

It returns Observables, making it reactive and asynchronous.

------------------------------------------------------------------------

## Setup

### 1️⃣ Import HttpClientModule

``` ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule {}
```

------------------------------------------------------------------------

# 1️⃣ GET Request (Read Data)

## Use Case

Fetching users from API.

``` ts
@Injectable({
  providedIn: 'root',
})
export class PostService {
  constructor( private http: HttpClient, @Inject(API_URL) private api: string) {}

  getPost(): Observable<Post[]> {
    return this.http.get<Post[]>(this.api);
  }
}
```

Component:

``` ts
@Component({
  selector: 'app-post',
  template: `
    <div *ngIf="post.length > 0">
      <div class="card" *ngFor="let item of post">
        <div class="card-body">
          <h4>Title : {{ item.title | uppercase }}</h4>
        </div>
        <div>Body : {{ item.body }}</div>

        <div class="card-footer">
          <small style="color: green;">Post Id :{{ item.id }}</small>
        </div>
      </div>
    </div>
  `,
})
export class PostComponent implements OnInit {
  post: Post[] = [];

  constructor(private postService: PostService) {}

  ngOnInit(): void {
    this.fetchPost();
  }

  fetchPost() {
    this.postService.getPost().subscribe({
      next: (data) => {
        this.post = data.slice(0, 9);
      },
      error: (err) => {
        alert('Something went wrong :' + err);
      },
    });
  }
}
```

### Ouptut
<img width="732" height="841" alt="image" src="https://github.com/user-attachments/assets/410f7aa7-1239-4d8c-b4ef-f10da8bd857a" />

------------------------------------------------------------------------

# 2️⃣ POST Request (Create Data)

## Use Case

Creating a new Post.

``` html

<div>
    <h2>Send Post Data to Server!</h2>
    <form class="form" #postForm="ngForm" (ngSubmit)="CreatePost(postForm)">

        <div class="form-group mb-2">
            <label for="title" class="form-label">Title : </label>
            <input class="form-control" type="text" name="title"
                id="title"
                #title="ngModel"
                [(ngModel)]="createPost.title" />
        </div>

        <div class="form-group mb-2">
            <label for="body" class="form-label">Body : </label>
            <input class="form-control" type="text" name="body"
                id="body"
                #body="ngModel"
                [(ngModel)]="createPost.body" />
        </div>

        <button class="btn btn-primary" type="submit">Save Post</button>

        <button class="btn btn-warning" type="button"
            (click)="ResetForm()">Reset Form</button>
    </form>
</div>
```

Component:

``` ts
export class PostComponent implements OnInit {
  post: Post[] = [];

  createPost:Partial<Post> = {
    id:12,
    title:'',
    body:'',
    userId:1
  };

  constructor(private postService: PostService) {}

  ngOnInit(): void {
    this.fetchPost();
  }

  fetchPost() {
    this.postService.getPost().subscribe({
      next: (data) => {
        this.post = data.slice(0, 9);
      },
      error: (err) => {
        alert('Something went wrong :' + err);
      },
    });
  }

  CreatePost(postForm:NgForm){
    
    if(postForm.valid){
    this.postService.createPost(this.createPost).subscribe({
      next:(data) => {
        alert("Submitted Successfully! Created Post Id" + data.id);
        this.ResetForm();
      },
      error:(err) => {
        alert(`Something went wrong ${err}`);
      }
    })
    }

  }

  ResetForm(){
    this.createPost = {title:'',id:1,body:'',userId:1}
  }
}

```

### Service Function

```ts
@Injectable({
  providedIn: 'root',
})
export class PostService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private api: string,
  ) {}

  createPost(newPost: Partial<Post>): Observable<Post> {
    return this.http.post<Post>(`${this.api}`, newPost);
  }
}
```

### Output

<img width="684" height="773" alt="image" src="https://github.com/user-attachments/assets/81fbbe1c-3971-4722-916d-d30fcf5efb93" />


------------------------------------------------------------------------

# 3️⃣ PUT Request 

## Use Case

``` ts
@Injectable({
  providedIn: 'root',
})
export class PostService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private api: string,
  ) {}

  getPost(): Observable<Post[]> {
    return this.http.get<Post[]>(this.api);
  }

  createPost(newPost: Partial<Post>): Observable<Post> {
    return this.http.post<Post>(`${this.api}`, newPost);
  }

  updatePost(id: number, updateData: Post): Observable<Post> {
    const url = `${this.api}/${id}`;
    return this.http.put<Post>(url, updateData);
  }
}

```

### Template

``` html

<div *ngIf="isEditing">
    <h2>Send Post Data to Server!</h2>
    <form class="form" #postForm="ngForm" (ngSubmit)="UpdatePost(postForm)">

        <div class="form-group mb-2">
            <label for="title" class="form-label">Title : </label>
            <input class="form-control" type="text" name="title"
                id="title"
                #title="ngModel"
                [(ngModel)]="editData.title" />
        </div>

        <div class="form-group mb-2">
            <label for="body" class="form-label">Body : </label>
            <input class="form-control" type="text" name="body"
                id="body"
                #body="ngModel"
                [(ngModel)]="editData.body" />
        </div>

        <button class="btn btn-primary" type="submit">Update Post</button>

        <button class="btn btn-warning" type="button"
            (click)="ResetForm()">Reset Form</button>
    </form>
</div>

<div class="card">
    <div *ngFor="let item of post">
        <div class="card-body">
            Title : {{item.title}}
        </div>

        <button class="btn btn-primary" (click)="editPost(item)">Edit
            Post</button>
    </div>

</div>
```

### Component Implementation
``` ts
export class PostComponent implements OnInit {
  post: Post[] = [];

  editData: Post = { body: '', title: '', id: 0, userId: 1 };

  isEditing: boolean = false;

  editPost(post: Post) {
    this.isEditing = true;

    this.editData = { ...post };
  }

  constructor(private postService: PostService) {}

  ngOnInit(): void {
    this.fetchPost();
  }

  fetchPost() {
    this.postService.getPost().subscribe({
      next: (data) => {
        this.post = data.slice(0, 9);
      },
      error: (err) => {
        alert('Something went wrong :' + err);
      },
    });
  }


  UpdatePost(postForm: NgForm) {
    if (postForm.valid) {
      const id = this.editData.id;
      const updateData = this.editData;

      this.postService.updatePost(id, updateData).subscribe({
        next: (data) => {
          if (data) {
            alert('Updated Successfully!, Updated Id :' + data.id);
            this.isEditing = false;
          }
        },
        error(err) {
          alert('something went wrong!' + err);
        },
      });
    }
  }

}
```


### Output

<img width="728" height="724" alt="image" src="https://github.com/user-attachments/assets/3bbd724b-b916-4806-8305-378ed2d28426" />

### 🔑 Key Implementation Details

- Cloning the Data: Using { ...post } (the spread operator) is important. It ensures that if the user starts typing in the edit box but clicks "Cancel," the original data in your list hasn't been changed.
- 
- The URL Construction: Your service correctly uses ${this.api}/${id}, which results in a call to https://jsonplaceholder.typicode.com/posts/1. The server needs this ID in the URL to know which specific record to replace.
- 
- PUT vs. PATCH: Remember that since you are using http.put, you must send the entire object. If you leave out the userId in your editData, the server might remove that field from the record.

------------------------------------------------------------------------

# 4️⃣ PATCH Request (Partial Update)

## Use Case

Updating only email.

``` ts
updateUserEmail(id: number, email: string) {
  return this.http.patch(
    `https://api.example.com/users/${id}`,
    { email }
  );
}
```

------------------------------------------------------------------------

# 5️⃣ DELETE Request (Remove Data)

## Use Case

Deleting user.

``` ts
deleteUser(id: number) {
  return this.http.delete(
    `https://api.example.com/users/${id}`
  );
}
```

------------------------------------------------------------------------

# Production-Level Service Example

``` ts
@Injectable({ providedIn: 'root' })
export class ProductService {

  private apiUrl = 'https://api.example.com/products';

  constructor(private http: HttpClient) {}

  getProducts() {
    return this.http.get<any[]>(this.apiUrl);
  }

  addProduct(product: any) {
    return this.http.post(this.apiUrl, product);
  }

  updateProduct(id: number, product: any) {
    return this.http.put(`${this.apiUrl}/${id}`, product);
  }

  deleteProduct(id: number) {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }

}
```

------------------------------------------------------------------------

# Best Practices (Production)

-   Always use services to handle HTTP logic.
-   Use strong typing instead of `any`.
-   Handle errors using `catchError`.
-   Use interceptors for auth tokens.
-   Avoid subscribing inside services.
-   Prefer async pipe when possible.

------------------------------------------------------------------------

## Key Takeaways

-   HttpClient handles all HTTP methods.
-   Returns Observables (reactive pattern).
-   Separate API logic into services.
-   Follow REST principles.
-   Essential for full-stack Angular applications.
