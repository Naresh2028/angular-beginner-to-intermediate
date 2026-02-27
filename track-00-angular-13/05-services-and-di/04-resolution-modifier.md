# Resolution Modifiers

## Definition

Resolution Modifiers in Angular Dependency Injection control how the
injector searches for dependencies.

They modify the default resolution behavior.

Main Modifiers:

-   @Optional() → Dependency may or may not exist
-   @Self() → Look only in the current injector
-   @SkipSelf() → Skip current injector and look in parent

These modifiers provide precise control over DI resolution behavior.

------------------------------------------------------------------------

## Analogy

Think of searching for a document in an office building:

-   @Self() → Check only your desk.
-   @SkipSelf() → Skip your desk and check your manager's desk.
-   @Optional() → If not found anywhere, don't throw an error.

------------------------------------------------------------------------

# @Optional()

## Definition

@Optional() tells Angular that if the dependency is not found, do NOT
throw an error. Instead, inject null.

------------------------------------------------------------------------

## Production-Level Example

### Scenario: Optional Analytics Service

``` ts
@Injectable()
export class AnalyticsService {
  track(event: string) {
    console.log("Tracking:", event);
  }
}
```

``` ts
@Component({
  selector: 'app-home',
  template: `<button (click)="click()">Click</button>`
})
export class HomeComponent {

  constructor(@Optional() private analytics: AnalyticsService) {}

  click() {
    if (this.analytics) {
      this.analytics.track("Home Clicked");
    }
  }

}
```

Production Use Case:

-   Feature-based services
-   Environment-specific services
-   Plugin-based architecture

------------------------------------------------------------------------

# @Self()

## Definition

@Self() forces Angular to look ONLY in the current injector
(component-level).

It will NOT check parent injectors.

------------------------------------------------------------------------

## Production-Level Example

``` ts
@Injectable()
export class FormStateService {
  state = {};
}
```

``` ts
@Component({
  selector: 'app-profile-form',
  template: `Profile Form`,
  providers: [FormStateService]
})
export class ProfileFormComponent {

  constructor(@Self() private formState: FormStateService) {}

}
```

Production Use Case:

-   Strict isolation of component services
-   Prevent accidental parent injection
-   Ensuring independent instances

------------------------------------------------------------------------

# @SkipSelf()

## Definition

@SkipSelf() tells Angular to skip the current injector and look in the
parent injector.

------------------------------------------------------------------------

## Production-Level Example

``` ts
@Injectable()
export class ThemeService {
  theme = "light";
}
```

``` ts
@Component({
  selector: 'app-parent',
  template: `<app-child></app-child>`,
  providers: [ThemeService]
})
export class ParentComponent {}
```

``` ts
@Component({
  selector: 'app-child',
  template: `Child`
})
export class ChildComponent {

  constructor(@SkipSelf() private theme: ThemeService) {
    console.log(this.theme.theme);
  }

}
```

Production Use Case:

-   Access parent configuration
-   Prevent child override usage
-   Multi-layer configuration systems

------------------------------------------------------------------------

# Key Differences

# Angular Dependency Injection Modifiers

| Modifier   | Search Behavior         | Error if Not Found |
|------------|--------------------------|--------------------|
| @Optional  | Normal search            | No (returns null)  |
| @Self      | Current injector only    | Yes                |
| @SkipSelf  | Parent injectors only    | Yes                |


------------------------------------------------------------------------

## Important Notes

-   These modifiers give fine-grained DI control.
-   Useful in complex enterprise architectures.
-   Prevent accidental service shadowing.
-   Improve predictability in large component trees.
-   Commonly used in library and framework-level development.

