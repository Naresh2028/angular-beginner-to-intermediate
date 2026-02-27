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

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'https://api.example.com/users';

  constructor(private http: HttpClient) {}

  getUsers(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl);
  }

}
```

### Using Service in Component

``` ts
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-users',
  template: `
    <ul>
      <li *ngFor="let user of users">
        {{ user.name }}
      </li>
    </ul>
  `
})
export class UsersComponent implements OnInit {

  users: any[] = [];

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userService.getUsers().subscribe(data => {
      this.users = data;
    });
  }

}
```

------------------------------------------------------------------------

## Key Takeaways

-   DI promotes loose coupling.
-   Injector resolves dependencies automatically.
-   `providedIn: 'root'` enables tree-shaking.
-   Services centralize reusable logic.
-   Metadata controls service lifecycle and scope.


