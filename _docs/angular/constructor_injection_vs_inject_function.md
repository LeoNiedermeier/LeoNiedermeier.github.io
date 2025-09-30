---
title: constructor injection vs `inject()`
tags: [angular]
category: angular
summary: "Here’s a concise, team‑friendly guideline for when to use **constructor injection** vs **`inject()`** in Angular (v16+ through v20), aligned with standalone components and typed DI."
---


Here’s a concise, team‑friendly guideline for when to use **constructor injection** vs **`inject()`** in Angular (v16+ through v20), aligned with standalone components and typed DI.

---

## TL;DR

- **Default to constructor injection** for components, directives, pipes, and services—keeps dependencies explicit and easy to test.  
- **Use `inject()`** where constructors are awkward or unavailable: field initializers, base classes to avoid `super(...)` churn, provider/`InjectionToken` factories, router guards/resolvers, and when you need `runInInjectionContext(...)`.  
- **Never** build a global service locator (e.g., stash `Injector` in a static). `inject()` is **contextual DI**, not a locator.  
  *Refs: Angular DI docs; `inject()` API & injection context; Fowler’s DI definition* [^1][^2][^3][^4]

---

## 1) Principles (why this guideline)

- **It’s all DI**: both constructor injection and `inject()` are still dependency injection in Fowler’s sense—dependencies are supplied by the container, not constructed or globally looked up by the class. [^4]  
- **`inject()` is contextual**: it only works in an **injection context** (e.g., class creation by Angular, field initializers of DI‑created classes, provider/`InjectionToken` factories, router guards; otherwise `NG0203`). [^2][^1]  
- **Explicit > implicit**: constructor parameters advertise dependencies up front; excessive `inject()` use can hide them and hinder test ergonomics. Use it judiciously. [^3]

---

## 2) Decision guide

### Prefer **constructor injection** when…

1) **Angular‑decorated classes with stable deps**  
   Components, directives, pipes, and services with a predictable set of dependencies.  
   ```ts
   @Component({standalone: true, /* … */})
   export class ProfileComponent {
     constructor(private readonly user: UserService) {}
   }
   ```
   *Why:* explicit API surface, straightforward unit tests (`new Class(mock)`), and familiar to most devs. [^1][^2]

2) **You want maximum dependency visibility**  
   Code reviews should see what the class needs at a glance (esp. in app-critical or shared libs).

3) **You instantiate classes outside Angular**  
   If something may be constructed directly in tests/tools, constructor params are simpler.

---

### Prefer **`inject()`** when…

1) **Field initializer ergonomics**  
   You want to avoid boilerplate constructors:  
   ```ts
   @Injectable({providedIn: 'root'})
   export class FeatureService {
     private readonly http = inject(HttpClient);
   }
   ```
   *Note:* still runs in DI context because Angular creates the class. [^1][^2]

2) **Inheritance without constructor churn**  
   Base classes can `inject()` their own deps, so subclasses don’t have to call `super(...)`. [^3]  
   ```ts
   abstract class BaseVm {
     protected readonly store = inject(Store);
   }
   export class OrdersVm extends BaseVm {
     // no super(store) noise
   }
   ```

3) **Functional/Factory APIs**  
   Router guards/resolvers, provider factories, and `InjectionToken` factories:  
   ```ts
   export const canActivateTeam: CanActivateFn = () => {
     const auth = inject(AuthService);
     return auth.isAllowed();
   };
   ```  
   ```ts
   export const API_URL = new InjectionToken<string>('API_URL', {
     factory: () => inject(ConfigService).apiUrl
   });
   ```  
   *These are designed to run in an injection context.* [^2]

4) **You need `runInInjectionContext`**  
   To execute code that requires DI while you hold an `EnvironmentInjector`:  
   ```ts
   const env = inject(EnvironmentInjector);
   runInInjectionContext(env, () => {
     const logger = inject(Logger);
     logger.info('within context');
   });
   ```  
   *This is the official way to establish an injection context manually.* [^2]

5) **Optional/self/host/skipSelf flags inline**  
   `inject(TOKEN, {optional: true, self: true})` replaces multiple parameter decorators. [^1]

---

## 3) Anti‑patterns to avoid

- **Global service locator**  
  Do **not** store a static `Injector` and call `get()` from anywhere. That hides dependencies and breaks DI clarity. `inject()` already enforces context; keep it that way. [^4][^2]

- **Scattered `inject()` calls inside methods**  
  Keep all `inject()` calls **at top-level fields** in the class. Don’t dynamically inject inside loops or deep in methods; it hides deps and complicates tests. [^3]

- **Calling `inject()` outside context**  
  If you’re not in a DI context, use `runInInjectionContext(envInjector, () => ...)`. [^2]

---

## 4) Testing guidance

- **Constructor injection classes**  
  Prefer plain unit tests: `const svc = new Class(mockDep as any)`; no `TestBed` needed.

- **Classes using `inject()`**  
  Use Angular testing utilities to provide the injection context:  
  ```ts
  TestBed.configureTestingModule({ providers: [Logger] });
  const logger = TestBed.inject(Logger); // or create component/fixture
  ```
  For factory/functional code, wrap with `runInInjectionContext(TestBed, () => ...)` (or use a fixture) to ensure a context exists. [^2][^1]

---

## 5) Style conventions

- **Placement**: put all `inject(...)` fields at the **top** of the class, after the decorator.  
- **Naming**: `private readonly http = inject(HttpClient)` (lowerCamelCase, `readonly`).  
- **Ordering**: framework/platform services → app services → tokens.  
- **Optional deps**: use `inject(Token, { optional: true })` and handle `null` paths explicitly. [^1]  
- **No mixed patterns just for style**: don’t split the same dependency between constructor and `inject()`.

---

## 6) Quick reference

| Situation | Use | Rationale |
|---|---|---|
| Component/directive/pipe/service with stable deps | **Constructor** | Explicit deps, easy unit tests, standard practice. [^1][^2] |
| Base class (avoid `super(...)`) | **`inject()`** | Cleaner inheritance. [^3] |
| Router guard/resolver, provider/`InjectionToken` factory | **`inject()`** | These APIs run in DI context by design. [^2] |
| Code needs DI but you only have an `EnvironmentInjector` | **`runInInjectionContext` + `inject()`** | Establishes context on demand. [^2] |
| Utils or classes constructed manually (outside Angular) | **Constructor** | No DI context; keeps tests trivial. [^4] |

---

## 7) Examples

**Constructor first, simple service**
```ts
@Injectable({ providedIn: 'root' })
export class OrdersService {
  constructor(private readonly http: HttpClient) {}
}
```

**Base class using `inject()`**
```ts
abstract class BaseEffects {
  protected readonly actions$ = inject(Actions);
  protected readonly store    = inject(Store);
}

@Injectable({ providedIn: 'root' })
export class OrdersEffects extends BaseEffects {
  // no constructor churn
}
```

**Router guard with `inject()`**
```ts
export const canActivateOrders: CanActivateFn = () => {
  const auth = inject(AuthService);
  return auth.canAccess('orders');
};
```

**Factory with `runInInjectionContext`**
```ts
@Injectable({ providedIn: 'root' })
export class ReportsService {
  private readonly env = inject(EnvironmentInjector);
  runTask() {
    runInInjectionContext(this.env, () => {
      inject(Logger).info('task running');
    });
  }
}
```

---

## 8) Team checklist (PR/review)

- [ ] Dependencies are visible (constructor or top‑level `inject()` fields only).  
- [ ] No `inject()` calls inside methods/loops.  
- [ ] No static/global `Injector` access or “service locator” helpers.  
- [ ] Tests don’t overuse `TestBed` when constructor DI would suffice.  
- [ ] `runInInjectionContext` used only when truly outside an injection context.  
- [ ] Optional dependencies are marked and handled.

---

If you want, I can turn this into an **ESLint rule set** (or Angular ESLint config) that flags disallowed `inject()` placements and static `Injector` usage.

---

### References

[^1]: Angular API – **`inject()`** (usage, options, contexts). <https://angular.dev/api/core/inject>  
[^2]: Angular Guide – **Injection context** (`NG0203`, router guards, `runInInjectionContext`, field initializers). <https://angular.dev/guide/di/dependency-injection-context>  
[^3]: “Angular’s `inject()` a couple of years later…” (pros/cons, inheritance ergonomics, visibility concerns). <https://dev.to/this-is-angular/angulars-inject-function-a-couple-of-years-later-5gl1>  
[^4]: Martin Fowler – **Inversion of Control Containers and the Dependency Injection pattern** (DI definition, forms, vs Service Locator). <https://martinfowler.com/articles/injection.html>
