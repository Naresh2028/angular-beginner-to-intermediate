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


## Purpose
GET is used to **retrieve data** from the server without modifying it.

## Signature

```ts
get<T>(url: string, options?: Object): Observable<T>
```

- url: endpoint to send the request

- options: headers, params, etc.

- Returns: an Observable<T> that you can subscribe to

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

## Purpose
POST is used to **create a new resource** on the server.

## Signature

```ts
post<T>(url: string, body: any, options?: Object): Observable<T>
```

- url: endpoint to send the request

- body: the data you want to create

- options: headers, params, etc.

- Returns: an Observable<T> that you can subscribe to

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

## Purpose
PUT is used to **update or replace** an existing resource.

## Signature

```ts
put<T>(url: string, body: any, options?: Object): Observable<T>
```

- url: endpoint to send the request

- body: the data you want to update

- options: headers, params, etc.

- Returns: an Observable<T> that you can subscribe to

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

We use a conditional *ngIf to show the edit form only when a post is selected.

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

## Template Implementation

````html
<div *ngIf="isEditing">
    <h2>Send Post Data to Server!</h2>
    <form class="form" #postForm="ngForm" (ngSubmit)="PartialEdit(postForm)">

        <div class="form-group mb-2">
            <label for="title" class="form-label">Title : </label>
            <input class="form-control" type="text" name="title"
                id="title"
                #title="ngModel"
                [(ngModel)]="currentPost.title" />
        </div>

        <div *ngIf="!hidediv" class="form-group mb-2">
            <label for="body" class="form-label">Body : </label>
            <input class="form-control" type="text" name="body"
                id="body"
                #body="ngModel"
                [(ngModel)]="currentPost.body" />
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

        <button class="btn btn-primary" type="button" (click)="editPost(item)">Edit
            Post
        </button>

        <button class="btn btn-warning" type="button" (click)="partialUpdate(item)">Partial Title Edit</button>
        <button class="btn btn-danger" type="button" (click)="DeletePost(item.id)">Delete Post</button>
    </div>

</div>
````

- Setting the Mode (hidediv): We use a simple true/false switch to tell the HTML what to show.

- For PUT (Edit Post): We set hidediv = false because we want the user to see and edit everything (Title and Body).

- For PATCH (Partial Edit): We set hidediv = true to hide the Body field, signaling that we only intend to change the Title.

## post.Complement.ts

``` ts
export class PostComponent implements OnInit {
  post: Post[] = [];
 hidediv: boolean = false;

  currentPost: Post = { body: '', title: '', id: 0, userId: 1 };

  isEditing: boolean = false;

  editPost(post: Post) {
    this.isEditing = true;

    this.currentPost = { ...post };
  }

  partialUpdate(post: Post) {
    this.isEditing = true;
    this.hidediv = true;
    this.currentPost = { ...post };
  }

  PartialEdit(postForm: NgForm) {
    if (postForm.valid) {
      const id = this.currentPost.id;
      const updateData = { title: this.currentPost.title };

      this.postService.PartialEdit(id, updateData).subscribe({
        next: (updatedList) => {
          if (updatedList) {
            this.hidediv = true;
            alert('The title updated successfully :' + updatedList.title);

            const index = this.post.findIndex((x) => x.id === id);
            this.isEditing = false;

            if (index !== -1) {
              this.post[index].title = updatedList.title;
            }
            this.ResetForm();
          }
        },
      });
    }
  }
}
```

###   What we did 

- Cloning the Data ({...post}): We don't want to change the text on the main list while the user is typing, in case they decide to click "Cancel". By using the "spread operator" (...), we create a temporary copy of the data for the user to edit.

- Selective Data Payload: In your component, We created a specific object updateData = { title: ... } to ensure you weren't sending unnecessary fields like body or -userId over the network.

- Optimistic UI Sync: Instead of reloading the whole list, we used findIndex() to locate the post in your local array and updated only its title property. This makes the update feel instantaneous to the user.

- Unified Form Logic: We used a single currentPost object for both full and partial edits, using a hidediv flag to conditionally show or hide form fields.   

## post.service.ts

```ts
  PartialEdit(id:number,updateRecord:Partial<Post>):Observable<Post>{
    const url = `${this.api}/${id}`
    return this.http.patch<Post>(url,updateRecord);
  }
````

### Ouptut
<img width="640" height="683" alt="image" src="https://github.com/user-attachments/assets/e3f28b05-8d92-4b20-9cb0-4f7546574a9d" />

### Summary

- Created a service method using http.patch rather than http.put. 
- While PUT replaces the entire resource, PATCH tells the server to only modify specific fields.

------------------------------------------------------------------------

# 5️⃣ DELETE Request (Remove Data)

## Template implementation

```html
<div class="card">
    <div *ngFor="let item of post">
        <div class="card-body">
            Title : {{item.title}}
        </div>

        <button class="btn btn-primary" type="button" (click)="editPost(item)">Edit
            Post
        </button>

        <button class="btn btn-warning" type="button" (click)="partialUpdate(item)">Partial Title Edit</button>
        <button class="btn btn-danger" type="button" (click)="DeletePost(item.id)">Delete Post</button>
    </div>

</div>
````

## Post.component.ts

```ts
post: Post[] = [];

  constructor(private postService: PostService) {}

  DeletePost(id: number) {
    if (confirm('Are you sure you want to delete this post?')) {
      this.postService.DeleteRecord(id).subscribe({
        next: (data0) => {
          alert('Deleted Successfully');

          this.post = this.post.filter(x => x.id !== id)
        },
        error: (err) => {
          alert('something went wrong...' + err);
        },
      });
    }
  }
````

### What we did

- User Confirmation: We added a confirm() dialog, which is a best practice to prevent accidental data loss.

- Client-Side Filtering: Since you are using a fake API (JSONPlaceholder) that doesn't actually delete data from its database, we used this.post.filter() to remove the item from the local array.

- Manual Synchronization: By updating the local this.post variable within the next block of the subscription, we ensured the UI re-rendered automatically to show the updated list without the deleted item.

## Post.service.ts

``` ts
   DeleteRecord(id:number):Observable<void>{
    return this.http.delete<void>(`${this.api}/${id}`);
  }
```

## Output

<img width="679" height="789" alt="image" src="https://github.com/user-attachments/assets/f8b680c6-0155-4b5a-b780-3eab42e944e6" />


------------------------------------------------------------------------

# Completed Production-Level Service Example

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

  PartialEdit(id:number,updateRecord:Partial<Post>):Observable<Post>{
    const url = `${this.api}/${id}`
    return this.http.patch<Post>(url,updateRecord);
  }

  DeleteRecord(id:number):Observable<void>{
    return this.http.delete<void>(`${this.api}/${id}`);
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
