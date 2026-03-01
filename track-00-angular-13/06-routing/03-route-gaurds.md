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

### Scenario: Protect Admin Route

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
