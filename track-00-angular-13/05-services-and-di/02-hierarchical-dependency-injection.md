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

``` ts
@Injectable({
  providedIn: 'root'
})
export class AuthService {}
```

-   One instance across the entire application.

------------------------------------------------------------------------

### 2️⃣ Component Level Service

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
