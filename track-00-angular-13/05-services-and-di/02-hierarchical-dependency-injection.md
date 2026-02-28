# Hierarchical Dependency Injection

## Definition

Hierarchical Dependency Injection means Angular creates a tree of
injectors.

Each level (Root, Module, Component) can have its own injector.

Scopes:

-   Root Level → Single instance shared across entire app
-   Module Level → Shared within a feature module
-   Component Level → New instance per component

This allows fine-grained control over service scope and lifecycle.

------------------------------------------------------------------------

## Analogy

Think of a company structure:

-   Head Office → Global policies (Root Injector)
-   Department → Department-specific tools (Module Injector)
-   Individual Team → Team-specific resources (Component Injector)

Each level can provide its own version of a service.

------------------------------------------------------------------------

## Production-Level Example

### 1️⃣ Root Level Service (Singleton)

By default, when you generate a service in Angular 13, it is configured as a singleton.

- One Instance: Angular creates only one instance of the service class for the entire app.
- Shared State: Because every component receives the same instance, they all share the same data. If Component A changes a variable in the service, Component B sees that change immediately.
- Memory Efficiency: Since there is only one object in memory, it prevents the overhead of creating duplicate objects for the same logic.

``` ts
@Injectable({
  providedIn: 'root'
})
export class AuthService {}
```

-   One instance across the entire application.

------------------------------------------------------------------------

### 2️⃣ Component Level Service

While services are singletons by default, Angular allows you to break this pattern using Hierarchical Dependency Injection.

- Component-Level Providers: If you list a service in a component's providers: [] array, Angular creates a new, private instance just for that component and its children.

- Multiple Instances: If you have five instances of a component on a page and each provides the service locally, you will have five separate instances of that service instead of one shared singleton.

``` ts
@Component({
  selector: 'app-editor',
  template: `Editor Component`,
  providers: [EditorStateService]
})
export class EditorComponent {}
```

``` ts
@Injectable()
export class EditorStateService {
  state = {};
}
```

Each EditorComponent gets its own EditorStateService instance.

Production Use Case:

-   Independent form states
-   Isolated widget configurations
-   Multi-instance dashboards

------------------------------------------------------------------------
