# RATE LIMITING (SEARCH FUNCTIONALITY)

In modern web applications, Search Functionality is the bridge between a user's intent and your data. It allows users to filter through large datasets (like products, posts, or users) by typing a keyword.

However, a "naive" search—where every single character typed triggers an API call—is a performance nightmare. It wastes server resources, drains mobile data, and can lead to "Race Conditions" where the UI shows the wrong results.

### We achieve this by using three core operators in a specific sequence:

#### 1. debounceTime(300) — The "Patience" Operator
What it does: It waits for a specific pause in the stream (e.g., 300ms) before letting the data pass through.

Why it's optimistic: It assumes the user isn't finished typing yet. By waiting, it consolidates multiple keystrokes into a single request.

The Result: If you type "Angular" in 200ms, only one request is sent at the end, instead of seven individual requests.

#### 2. distinctUntilChanged() — The "Memory" Operator
What it does: it compares the current value with the previous one. If they are identical, it blocks the emission.

Why it's optimistic: It assumes that if the search term hasn't changed, the results won't change either.

The Result: If a user types "Search", hits backspace, then re-types "h" quickly, the final value is still "Search." This operator prevents a second, redundant API call for the same word.

#### 3. switchMap() — The "Freshness" Operator
What it does: When a new search term arrives, it cancels any previous HTTP request that is still "in-flight" and switches to the new one.

Why it's optimistic: It prioritizes the latest user intent. It assumes the user only cares about the results for what they just typed, not what they typed a second ago.

The Result: This eliminates "Race Conditions." You will never see results for "Apples" pop up after you've already moved on to searching for "Oranges."


## 1. The Service (data.service.ts)

We will use the title_like query parameter to tell the server to find any title that contains the letters the user types.

````ts
@Injectable({
  providedIn: 'root',
})
export class SearchService {
  constructor(
    @Inject(API_URL) private apiUrl: string,
    private http: HttpClient,
  ) {}

  searchPost(text: string): Observable<any> {
    if (!text) {
      return of([]);
    }

    const params = new HttpParams().set('title_like', text);
    return this.http.get<any>(this.apiUrl, { params });
  }
}
````

### What happens here

- of([]): If the user clears the search box, we return an empty array immediately. We don't want to waste the user's data or our server's resources fetching "nothing."

- HttpParams: Hardcoding strings like url + '?q=' + term is dangerous because special characters (like & or #) can break the URL. HttpParams handles the encoding for you.

- Type Safety: By returning Observable<any[]>, you tell the component exactly what to expect, allowing the AsyncPipe to render the list safely.

## 2. The Component (search.component.ts)

This component uses a Subject to handle the typing. It applies the "Rate Limiting" operators we discussed to make the search smooth.

````ts
export class SearchComponent implements OnInit {
  private SearchText = new Subject<string>();

  posts$!: Observable<any>;

  constructor(private searchService: SearchService) {}

  ngOnInit(): void {
    this.posts$ = this.SearchText.pipe(
      debounceTime(300),
      distinctUntilChanged(),
      switchMap((text: string) => this.searchService.searchPost(text)),
    );
  }

  Search(text: string) {
    this.SearchText.next(text);
  }
}
````


### 🚀 Why these are "Performance Heroes"

- debounceTime(400): This prevents "flicker-ing." If a user types at 100 words per minute, you only send one request at the very end of their sentence, rather than 50 separate requests.

- distinctUntilChanged(): This handles the "Oops" factor. If a user types "Angular", then deletes the last 'r' and quickly types 'r' again (returning to "Angular"), this operator realizes the value hasn't actually changed and blocks the duplicate API request.

## 3. The Template (search.component.html)

This is where you see the output. We use the AsyncPipe to subscribe to the results and display them in a list.
````html
<div>
    <input #searchBox
        placeholder="Type to Search posts"
        (input)="Search(searchBox.value)">

    <div *ngIf="posts$ | async as posts; else initial">
        <ul class="list-group" *ngIf="posts.length > 0; else noResults">
            <li *ngFor="let post of posts">
                <h5>Title : {{post.title}}</h5>
                <p>Body : {{post.body}}</p>
            </li>
        </ul>
    </div>

    <ng-template #noResults>
        <div class="alert alert-warning">No posts match your search.</div>
    </ng-template>

    <ng-template #initial>
        <p class="text-muted">Start typing to see results...</p>
    </ng-template>
</div>
````

### Output

<img width="660" height="930" alt="image" src="https://github.com/user-attachments/assets/8415968e-dc73-49ef-83ef-ee2ede6cac8e" />



### Summary of the Flow

User types: "sunt"

Service: Checks !text.trim(). It is False, so it skips the if.

HTTP: Sends request to apiUrl?title_like=sunt.

Template: Receives real data, and the "No posts match" message disappears!





