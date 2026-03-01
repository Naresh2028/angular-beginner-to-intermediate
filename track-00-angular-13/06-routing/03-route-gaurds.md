# Route Guards

## Definition

Route Guards are services that control navigation to or from a route.

They decide whether a route should be:

-   Activated (CanActivate)
-   Deactivated (CanDeactivate)
-   Loaded (CanLoad)
-   Matched (CanMatch)

Guards improve security and user flow control.

------------------------------------------------------------------------

## Analogy

Think of airport security.

-   Before boarding a flight, your credentials are verified.
-   If you don't meet requirements, access is denied.

Route Guards protect routes the same way.

------------------------------------------------------------------------

## Production-Level Example

### Step 1: Create Guard

### 1. CanActivate: Authentication Guard

This is the most common guard. It checks if a user is logged in before allowing them to enter a route like /dashboard.

``` ts
import { Injectable } from '@angular/core';
import {
  ActivatedRouteSnapshot,
  CanActivate,
  Router,
  RouterStateSnapshot,
  UrlTree,
} from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class AuthGuard implements CanActivate {
  isLoggedIn!: string | null;

  constructor(private route: Router) {}

  canActivate(): boolean {
    this.isLoggedIn = localStorage.getItem('userToken');

    if (this.isLoggedIn) {
      return true;
    } else {
      alert('you are not authiorized credentials to access, please login...');
      this.route.navigate(['/']);
      return false;
    }
  }
}

```

------------------------------------------------------------------------

### Step 2: Apply Guard in Routing

``` ts
const routes: Routes = [
  { path: '', component: LoginComponent, pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent,canActivate:[AuthGuard] },
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
### Component Implementation

````ts
export class LoginComponent implements OnInit {
  constructor(private route: Router) {}

  ngOnInit(): void {}

  loginData = {
    username: '',
    password: '',
  };

  onSubmit() {
    if (this.loginData.username && this.loginData.password ) {
      alert('Login attempted:' + JSON.stringify(this.loginData));
      localStorage.setItem('userToken', 'naresh');
      this.route.navigate(['/dashboard']);
    } 
  }
}
````

### Output

<img width="1622" height="822" alt="image" src="https://github.com/user-attachments/assets/f0a9963e-e476-40eb-808a-97c080045d18" />

---

#### Summart
- CanActivate is used to control entry (Authentication/Authorization).

---

### 2. CanDeActivate : Unsaved Changes Guard

This guard prevents users from accidentally navigating away from a form (like UserNewComponent) if they have typed data but haven't clicked "Save".

### Guard Implementation

````ts
export interface canComponentDeactivate{
  canDeactivate:() => boolean
}

@Injectable({
  providedIn: 'root'
})
export class UnsavedChangesGuard implements CanDeactivate<canComponentDeactivate> {
  canDeactivate(component: canComponentDeactivate): boolean {
    return component.canDeactivate ? component.canDeactivate() : true;
  }
}
````
- export interface canComponentDeactivate → Defines a contract that any component can implement if it wants to support the “unsaved changes” check.

- canDeactivate: () => boolean → Requires the component to have a method called canDeactivate that returns a boolean (true = allow navigation, false = block navigation).

- UnsavedChangesGuard → The guard class itself.

- implements CanDeactivate<canComponentDeactivate> → Tells Angular this guard will run before deactivating a route, and it applies to components that implement the canComponentDeactivate interface.

- Checks if the component has a canDeactivate method.

- If yes → calls it, and the component decides whether navigation is allowed.

- If not → defaults to true (navigation allowed).

````ts
export class UserNewComponent implements canComponentDeactivate {
  userName = '';
  constructor(private route: Router) {}

  SaveUser() {
    alert(`New User Added ${this.userName}`);
    this.route.navigateByUrl('/users');
  }

  canDeactivate(): boolean {
    if (this.userName.length > 0) {
      return confirm('You have unsaved changes! Do you really want to leave?');
    }
    return true;
  }
}
````

### Output

<img width="1405" height="766" alt="image" src="https://github.com/user-attachments/assets/4c67e8c5-4dbb-4c54-84b3-51534136eca1" />

---

# Angular Route Guards

| Guard Type   | Purpose           | Main Use Case                                      |
|--------------|------------------|---------------------------------------------------|
| CanActivate  | Entrance Control | Checking if a user is logged in or is an Admin     |
| CanDeactivate| Exit Control     | Preventing data loss on forms or stopping a video/upload |

------------------------------------------------------------------------

Production Use Cases:

-   Authentication & Authorization
-   Role-based access control
-   Prevent leaving unsaved forms (CanDeactivate)
-   Lazy-loaded module protection

------------------------------------------------------------------------

## Key Takeaways

-   Nested routes create hierarchical navigation.
-   Parent route must contain `<router-outlet>`{=html}.
-   Guards control route access and navigation flow.
-   Essential for security in enterprise apps.
-   Keep guard logic lightweight and delegate heavy logic to services.
