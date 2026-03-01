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

``` ts
import { HttpParams } from '@angular/common/http';

getUsers(page: number, search: string) {

  let params = new HttpParams()
    .set('page', page)
    .set('search', search);

  return this.http.get<User[]>(
    'https://api.example.com/users',
    { params }
  );
}
```

Generated URL:

    https://api.example.com/users?page=1&search=admin

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
