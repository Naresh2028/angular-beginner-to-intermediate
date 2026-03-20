# DEPRECATED FEATURES (Angular 15)

## List All the Deprecated Features

Angular 15 deprecated several legacy APIs and patterns to move toward a simpler, standalone-first architecture.

Major deprecated areas:

1. NgModule-based bootstrapping (discouraged)
2. RouterModule.forRoot / forChild (in standalone apps)
3. HttpClientModule (in standalone apps)
4. Class-based Route Guards (prefer functional guards)
5. Legacy TestBed APIs
6. View Engine remnants (removed direction)

---

# 1. NgModule-based Bootstrapping (Discouraged)

## Reason

Angular is moving toward **Standalone APIs**, reducing the need for NgModules.

### Example

❌ Old

@NgModule({
  bootstrap: [AppComponent]
})
export class AppModule {}

✅ New

bootstrapApplication(AppComponent);

---

# 2. RouterModule.forRoot / forChild

## Reason

Standalone routing replaces NgModule-based routing.

### Example

❌ Old

RouterModule.forRoot(routes)

✅ New

provideRouter(routes)

---

# 3. HttpClientModule

## Reason

Functional providers simplify setup.

### Example

❌ Old

imports: [HttpClientModule]

✅ New

provideHttpClient()

---

# 4. Class-based Route Guards

## Reason

Functional guards are simpler and cleaner.

### Example

❌ Old

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate() {
    return true;
  }
}

✅ New

export const authGuard = () => true;

---

# 5. Legacy TestBed APIs

## Reason

Modern Angular prefers `inject()`.

### Example

❌ Old

TestBed.get(Service)

✅ New

TestBed.inject(Service)

---

# 6. View Engine (Removed Direction)

## Reason

Angular fully uses Ivy.

### Example

enableIvy: false  ❌ not needed

---

# Key Notes

- Standalone architecture is the future
- NgModules are optional, not removed
- Prefer functional APIs
- Migration can be gradual

---

# Summary

Angular 15 deprecations focus on simplifying architecture and moving toward standalone APIs and functional patterns.
