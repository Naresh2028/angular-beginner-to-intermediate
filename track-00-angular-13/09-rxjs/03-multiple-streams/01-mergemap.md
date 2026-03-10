# MERGEMAP()

While switchMap() is the "Cancelation" operator, mergeMap() is the "Parallel" or "Don't Stop Me Now" operator. It is essential when you want every single event to finish its work, regardless of how many new events start.

## What is mergeMap()?

- mergeMap() (also known as flatMap) is a Transformation Operator that projects each source value into an internal Observable and merges them into one single output stream.

- Unlike switchMap, which kills the previous task to start a new one, mergeMap says: "I don't care if a new task started; I will keep the old ones running until they all finish." It allows multiple internal subscriptions to be active at the same time.


## Scenario: The "Delete" or "Save" Action

- Imagine an Admin Panel where you have a list of users. Next to each user is a "Delete" button.

- The Problem: If you use switchMap and click Delete on User A, then immediately click Delete on User B, switchMap will cancel the deletion of User A to focus on User B. User A might remain in your database!

- The Solution: Use mergeMap. If you click Delete 5 times on 5 different users, mergeMap triggers 5 parallel API calls. All 5 users will be deleted successfully, even if the clicks happen milliseconds apart.

## Example

##  The Service (user.service.ts)

````ts
export class UserService {
  constructor(
    private http: HttpClient,
    @Inject(API_URL) private url: string,
  ) {}

  deleteUser(id:number):Observable<any>{
    return this.http.delete(`${this.url}/${id}`);
  }
}
````

## 2. The Component (user-delete.component.ts)

We use a Subject to trigger the deletion and mergeMap to ensure every delete finishes.

````ts
@Component({
  selector: 'app-user-delete',
  templateUrl: './user-delete.component.html',
  styleUrls: ['./user-delete.component.css'],
})
export class UserDeleteComponent {
  deleteAction = new Subject<number>();

  deleteResult$ = this.deleteAction.pipe(
    mergeMap((id) => this.userServie.deleteUser(id)),
  );

  constructor(private userServie: UserService) {}

  OnDelete(id: number) {
    this.deleteAction.next(id);
    console.log(`Delete User Id ${id}`);
  }
}
````

## 3. The HTML (user-delete.component.html)
 
The AsyncPipe listens for the results of the deletions.

````html
<div class="p-3">
    <button class="btn btn-danger" (click)="OnDelete(1)">Delete User 1</button>
    <button class="btn btn-danger" (click)="OnDelete(2)">Delete User 2</button>
    <button class="btn btn-danger" (click)="OnDelete(3)">Delete User 3</button>

    <hr>
    <div *ngIf="deleteResult$ | async" class="alert alert-success">
        User deletion request processed successfully!
    </div>
</div>
````

## Output

<img width="1400" height="922" alt="image" src="https://github.com/user-attachments/assets/af41c883-117d-45b9-98f9-0644d16fdef5" />


### Key Notes for mergeMap()

- Parallelism: It handles multiple requests at once. If your .NET API takes 2 seconds to delete, and you click 3 times, you will see 3 "Pending" requests in your browser's Network tab simultaneously.

- Order is NOT Guaranteed: Since the requests run in parallel, if "Delete 2" is faster than "Delete 1," it will finish first. If you need the order to stay the same (1 then 2 then 3), you should use concatMap.

- Memory Warning: Because mergeMap never cancels anything, if you use it on a stream that never completes (like an interval), you can accidentally create thousands of active subscriptions.

- Best Use Case: "Write" operations (POST, PUT, DELETE) where every single action is important and must reach the server.


