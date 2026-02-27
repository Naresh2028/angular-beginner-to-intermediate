# Directives

## Definition

Directives in Angular are special instructions in the template that
modify the structure or behavior of DOM elements.

They allow you to:

-   Change appearance
-   Control rendering
-   Attach custom behavior to elements

Directives enhance HTML with dynamic capabilities.

------------------------------------------------------------------------

## Analogy

Think of traffic signals on a road.

-   They control movement.
-   They change behavior based on conditions.

Directives control how elements behave and appear in the UI.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Conditionally Displaying Content

``` html
<p *ngIf="isLoggedIn">Welcome Back!</p>
```

Here:

-   The paragraph is rendered only if `isLoggedIn` is true.
-   Angular modifies the DOM structure dynamically.

------------------------------------------------------------------------

## Types

Angular provides three main types of directives:

1.  Built-in Directive
2.  Structural Directives
3.  Attribute Directives

------------------------------------------------------------------------

# Built-in 

## Definition

Built-in directives are directives provided by Angular out of the box.

They help manipulate DOM structure and appearance without writing custom
logic.

Common built-in directives:

-   `*ngIf`
-   `*ngFor`
-   `ngSwitch`
-   `[ngClass]`
-   `[ngStyle]`

------------------------------------------------------------------------

## Analogy

Think of built-in tools in a toolkit.

-   You don't build them yourself.
-   You use them when needed.

Angular provides these directives ready to use.

------------------------------------------------------------------------

# Structural Directives

# \*ngIf

## Definition

`*ngIf` is a structural or built-in directive that conditionally adds or removes an
	element from the DOM based on a Boolean expression.

If the condition is true, the element is rendered. If false, it is
removed from the DOM.

------------------------------------------------------------------------

## Analogy

Think of a security gate.

-   If you have access → gate opens.
-   If you don't → gate stays closed.

`*ngIf` works the same way for UI elements.

------------------------------------------------------------------------

## Production-Level Example

``` ts
@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 class="card-title mb-3 text-primary">Angular Learning</h1>

      <button *ngIf="isAdmin" class="btn btn-primary" (click)="deleteUser()">
        Delete User
      </button>
    </app-card>
  `,
})
export class AppComponent {
  currentRole = {
    name: 'Naresh',
    role: 'Admin',
  };

  get isAdmin(): boolean {
    return this.currentRole.role === 'Admin';
  }

  deleteUser() {
    alert('Deleted successfully!');
  }
}
```
<img width="656" height="381" alt="image" src="https://github.com/user-attachments/assets/9bfaf6ea-ede4-4685-a9bb-06931f213f84" />

Use Case:

-   Role-based access control
-   Conditional rendering of UI elements
-   Showing loaders or error messages

------------------------------------------------------------------------

# \*ngFor

## Definition

`*ngFor` is a structural directive used to iterate over a collection and
render elements for each item.

------------------------------------------------------------------------

## Analogy

Think of a printer printing multiple copies.

For every item in the list, a new copy is created.

------------------------------------------------------------------------

## Production-Level Example

``` ts
interface User{
  id:number,
  name:string,
  email:string
}

@Component({
  selector: 'app-root',
  template: `
    <router-outlet></router-outlet>

    <app-card>
      <h1 class="card-title mb-3 text-primary">Angular Learning</h1>

      <table class="table">
        <thead>
          <th>S.No</th>
          <th>Name</th>
          <th>Email</th>
        </thead>
        <tbody *ngFor="let user of Users; index as i ">
          <td>{{i + 1}}</td>
          <td>{{user.name}}</td>
          <td>{{user.email}}</td>
        </tbody>
      </table>
    </app-card>
  `,
})
export class AppComponent implements OnInit {
  
  Users:User[] = [];

  ngOnInit(): void {
    this.Users = [
      { id: 101, name: 'Naresh', email: 'naresh@example.com' },
      { id: 102, name: 'Amit', email: 'amit@example.com' },
      { id: 103, name: 'Sita', email: 'sita@example.com' },
      { id: 104, name: 'John', email: 'john@example.com' }
    ];
  }
}
```

<img width="546" height="385" alt="image" src="https://github.com/user-attachments/assets/0d37622f-7be6-446d-b6c3-1fee3d10f198" />


Use Case:

-   Rendering API data lists
-   Displaying products, users, notifications
-   Generating dynamic tables

	------------------------------------------------------------------------

	# ngSwitch

	## Definition

	`ngSwitch` conditionally displays elements based on matching
	expressions.

	It works similar to a switch statement in programming.

	------------------------------------------------------------------------

	## Analogy

	Think of a railway track switch.

	Depending on the signal, the train moves in a specific direction.

	------------------------------------------------------------------------

	## Production-Level Example

	``` html
	<div [ngSwitch]="role">
	  <p *ngSwitchCase="'admin'">Admin Dashboard</p>
	  <p *ngSwitchCase="'user'">User Dashboard</p>
	  <p *ngSwitchDefault>Guest View</p>
	</div>
	```

	Use Case:

	-   Role-based dashboards
	-   Status-based UI
	-   Multi-condition rendering

	------------------------------------------------------------------------
	# Attribute Directives

	# \[ngClass\]

	## Definition

	`[ngClass]` is an attribute directive used to dynamically apply CSS
	classes to an element.

	------------------------------------------------------------------------

	## Analogy

	Think of wearing different uniforms based on your role.

	Your appearance changes depending on the situation.

	------------------------------------------------------------------------

	## Production-Level Example

	``` html
	<div [ngClass]="{ 'active': isActive, 'disabled': !isActive }">
	  Status Box
	</div>
	```

	Use Case:

	-   Conditional styling
	-   Form validation states
	-   Status indicators

	------------------------------------------------------------------------

	# \[ngStyle\]

	## Definition

	`[ngStyle]` is an attribute directive used to dynamically apply inline
	styles to an element.

	------------------------------------------------------------------------

	## Analogy

	Think of adjusting brightness and color on a smart screen dynamically.

	Styles change based on conditions.

	------------------------------------------------------------------------

	## Production-Level Example

	``` html
	<div [ngStyle]="{ 'color': isError ? 'red' : 'green', 'font-size.px': 18 }">
	  System Status
	</div>
	```

	Use Case:

	-   Dynamic theming
	-   Error highlighting
	-   Real-time UI feedback

	------------------------------------------------------------------------

	## Key Notes

	-   `*ngIf`, `*ngFor`, `ngSwitch` → Structural Directives (modify DOM
		structure)
	-   `[ngClass]`, `[ngStyle]` → Attribute Directives (modify appearance)
	-   Structural directives use `*` syntax
	-   Attribute directives use property binding `[]`
	-   These are optimized and widely used in production applications


