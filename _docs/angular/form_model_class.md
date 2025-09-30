---
title: Form model class (a20)
tags: [angular]
category: angular
summary: "Use a class frü form model"
---
# Notes & options from AI
- Scope control
  - Per-component instance (recommended): keep providers: [PersonFormModel] in the component as shown.
  - Singleton: use @Injectable({ providedIn: 'root' }) on the model and remove the providers array (but beware of shared form state across pages).

- Type of birthday:
  - Using native `<input type="date">` → keep as string (YYYY-MM-DD).
  - Using MatDatepicker + MatNativeDateModule → change the control to FormControl<Date>.
- Unit tests:
   - This refactor makes testing easier—instantiate PersonFormModel directly (no TestBed/DI required) for pure form logic, or use component provider for integration tests.

Here are the **practical benefits** of using the `FormGroup` / `FormControl` constructors instead of `FormBuilder`—especially with typed forms in Angular 16+:
  
## Less coupling to Angular DI (simpler construction & testing)
- **No service injection** required (`FormBuilder` is a service). You can create forms in plain TypeScript classes,  helper functions, or unit tests **without** `TestBed` or `inject(FormBuilder)`.
- This makes **library/util code** cleaner and makes unit tests faster and more focused (no DI setup for the form model).
## More explicit & readable control definitions
- Constructors make the **shape obvious**: the control value, validators, and options are separated and self-explanatory—no need to remember tuple positions like `[value, validators, asyncValidators]`.
- Async validators and options are clearer
## Stronger type discipline (Typed Forms)
- With constructors, you’re nudged to declare **generic types** explicitly, which guards you against accidental `null` or wrong shapes.
- At the group level you can fully type the form for **end-to-end IntelliSense**:
## Fewer hidden defaults & clearer nullability
- With `FormBuilder`, devs often rely on the tuple shorthand and unintentionally end up with `string | null` controls.
- Constructors make **nullability an explicit choice** via `nonNullable: true` or by typing `string | null`.
## Easier reuse & composition
- You can **compose controls** before grouping, reuse validators/configuration, and split large forms into smaller, reusable blocks without always threading a `FormBuilder` reference around. 
## Clearer migration path & consistency
- Using constructors aligns with the underlying API that `FormBuilder` wraps. If the API evolves, you’re closest to the source of truth.
- You avoid team-level ambiguity between tuple shorthand vs. object/constructor style—**one consistent pattern**.
   
~~~typescript
import {Injectable} from '@angular/core';
import {FormControl, FormGroup, Validators,} from '@angular/forms';
@Injectable({
  providedIn: 'root'
})
export class PersonFormModel {

 readonly form = new FormGroup({
    firstName: new FormControl<string>('', {
      nonNullable: true,
      validators: [Validators.required, Validators.minLength(2)],
    }),
    lastName: new FormControl<string>('', {
      nonNullable: true,
      validators: [Validators.required],
    }),
    birthday: new FormControl<Date | null>(null, {
      nonNullable: true,
      validators: [Validators.required],
    }),
  });
 
  markAllAsTouched(): void {
    this.form.markAllAsTouched();
  }

  reset(): void {
    this.form.reset();
  }
}


export type PersonFormGroup = PersonFormModel['form'];

export type PersonControlName = keyof PersonFormGroup['controls']
~~~
