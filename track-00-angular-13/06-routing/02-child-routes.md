# Nested (Child) Routes

## Definition

Nested Routes (Child Routes) allow you to define routes inside other
routes.

They enable hierarchical navigation where a parent component contains a
`<router-outlet>`{=html} for rendering its child components.

This is useful for building complex layouts like dashboards with
multiple sections.

------------------------------------------------------------------------

## Analogy

Think of a shopping mall.

-   The mall is the parent route.
-   Individual stores inside the mall are child routes.
-   You enter the mall first, then navigate to specific stores.

Parent layout stays constant, inner content changes.

------------------------------------------------------------------------

## Production-Level Example

### Routing Configuration

``` ts
const routes: Routes = [
  { path: '', component: HomeComponent, pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'settings', component: SettingsComponent },
  { path: 'profile', component: ProfileComponent },
  { path: 'logout', component: LogoutComponent },

  //Parent Route
  {
    path: 'users',
    component: UserlistComponent,
    children: [
      //Child Routes
      { path: 'new', component: UserNewComponent },
      { path: ':id', component: UserdetailComponent },
    ],
  },

  { path: '**', component: NotFoundComponentComponent },
];
```

------------------------------------------------------------------------

### Parent Route (User-List Component)

``` ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-userlist',
  template: `
    <ul>
      <li *ngFor="let user of users">
        <b>User Name :</b> <span>{{ user.name }}</span>
        <br />
        <a [routerLink]="[user.id]">View Users</a>
      </li>
    </ul>

    <button class="btn btn-primary" routerLink="new">Add New</button>

    <router-outlet></router-outlet>
  `,
})
export class UserlistComponent implements OnInit {
  constructor() {}

  ngOnInit(): void {}

  users = [
    { id: 1, name: 'Naresh', role: '.NET Developer' },
    { id: 2, name: 'Sandy', role: 'QA' },
    { id: 3, name: 'Monish', role: 'Devops' },
  ];
}

```

### Child Routes (User-Detail Component)

```ts

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-userdetail',
  template: `
    <h2>User Details</h2>
    <p>Selected Id value is {{ userId }}</p>
    <button class="btn btn-primary" routerLink="/users">Back to Users</button>
  `,
})
export class UserdetailComponent {
  userId!: string | null;

  constructor(private route: ActivatedRoute) {
    this.userId = this.route.snapshot.paramMap.get('id');
    alert('Fetched data from user:' + this.userId);
  }
}

````

### Child Routes (New-User Component)

```ts
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-user-new',
  template: `
    <h3>New User Creation</h3>

    <div>
      <label>User Name</label>
      <input
        [(ngModel)]="userName"
        name="userName"
        placeholder="Enter User Name"
      />

      <button (click)="SaveUser()">Add User</button>
      <button routerLink="/users">Back to User</button>
    </div>
  `,
})
export class UserNewComponent implements OnInit {
  userName = '';
  constructor(private route: Router) {}

  ngOnInit(): void {}

  SaveUser() {
    alert(`New User Added ${this.userName}`);

    this.route.navigateByUrl('/users');
  }
}

```

### Output

#### Add New User

<img width="1285" height="743" alt="image" src="https://github.com/user-attachments/assets/f96d43f1-1831-4190-b833-96837bdd7d65" />

#### View User Detail

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9acf97a5-293a-412f-a413-9add44e325d0" />

#### Summary

- For "View Users": Using [routerLink]="[user.id]" is correct for relative navigation from a parent to a child.

- For "Add New": Using routerLink="new" is also correct.

- Crucial Step: Ensure that your UserlistComponent.html contains the <router-outlet></router-outlet> tag. Without this tag, Angular has no place to render the child components, which can also lead to routing failures.

------------------------------------------------------------------------

Production Use Cases:

-   Admin dashboards
-   Settings pages with tabs
-   Multi-step workflows
-   Feature module routing

------------------------------------------------------------------------

