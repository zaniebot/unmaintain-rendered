```yaml
number: 14238
title: "[`flake8-pyi`] Add \"replace with `Self`\" fix (`PYI019`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: PYI019
created_at: 2024-11-10T03:03:39Z
updated_at: 2024-11-12T12:10:42Z
url: https://github.com/astral-sh/ruff/pull/14238
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Add "replace with `Self`" fix (`PYI019`)

---

_@InSyncWithFoo_

## Summary

Resolves #14183.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-10 03:03_

---

_@InSyncWithFoo reviewed on 2024-11-10 03:06_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:361 on 2024-11-10 03:06_

Is there an `Expr` visitor I can use? I really don't feel like writing one myself.

---

_Comment by @github-actions[bot] on 2024-11-10 03:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2024-11-11 10:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:324 on 2024-11-11 10:23_

Nit:
```suggestion
    let (edit, ..) = importer.get_or_import_symbol(&request, position, semantic).ok()?;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:361 on 2024-11-11 10:27_

I'm not sure if we can fix complex annotations because the annotation, or at least, not all of them. For example, we can't fix: 

```
a: B | "TypeVar[A]"
```

because it would require parsing the string literal. Or at least, I don't think it's worth it. 


You could consider implementing your own `Visitor` but I'm not sure if it's worth the complexity. Maybe just handle the few common cases that you care about?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:390 on 2024-11-11 10:27_

```suggestion
    let is_declaration_in_question = |type_param: &TypeParam| -> bool {
```

---

_@MichaReiser reviewed on 2024-11-11 10:28_

---

_@InSyncWithFoo reviewed on 2024-11-11 13:02_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:361 on 2024-11-11 13:02_

We are only suggesting the fix for `.pyi` files, so it's probably safe to assume there are no string annotations (`PYI020` will deal with those). I agree that it might be unnecessarily complex, so let's just mark the fix as unsafe when an annotation cannot be handled.

---

_@InSyncWithFoo reviewed on 2024-11-11 13:09_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:390 on 2024-11-11 13:09_

This seems to lead to a compiler error:

```text
error[E0631]: type mismatch in closure arguments
    --> crates\ruff_linter\src\rules\flake8_pyi\rules\custom_type_var_return_type.rs:404:24
     |
386  |     let is_declaration_in_question = |type_param: &TypeParam| -> bool {
     |                                      -------------------------------- found signature defined here
...
404  |         .find_position(is_declaration_in_question)?;
     |          ------------- ^^^^^^^^^^^^^^^^^^^^^^^^^^ expected due to this
     |          |
     |          required by a bound introduced by this call
     |
     = note: expected closure signature `for<'a> fn(&'a &ruff_python_ast::TypeParam) -> _`
                found closure signature `fn(&ruff_python_ast::TypeParam) -> _`
note: required by a bound in `find_position`
    --> ...\itertools-0.13.0\src\lib.rs:1974:34
     |
1972 |     fn find_position<P>(&mut self, mut pred: P) -> Option<(usize, Self::Item)>
     |        ------------- required by a bound in this associated function
1973 |     where
1974 |         P: FnMut(&Self::Item) -> bool,
     |                                  ^^^^ required by this bound in `Itertools::find_position`
help: consider wrapping the function in a closure
     |
404  |         .find_position(|arg0: &&ruff_python_ast::TypeParam| is_declaration_in_question(*arg0))?;
     |                        ++++++++++++++++++++++++++++++++++++                           +++++++
help: consider adjusting the signature so it borrows its argument
     |
386  |     let is_declaration_in_question = |type_param: &&TypeParam| -> bool {
     |                                                   +
```

---

_@MichaReiser reviewed on 2024-11-11 13:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:390 on 2024-11-11 13:20_

Fair enough, but we can use the result of `find_position` rather than accessing the element by `index `again

```
let (index, declaration) = parameters
        .into_iter()
        .find_position(is_declaration_in_question)?;

    let typevar_range = declaration.as_type_var().unwrap().range();
    let last_index = parameters.len() - 1;
```

---

_@MichaReiser reviewed on 2024-11-11 13:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:311 on 2024-11-11 13:22_

To avoid relocating the entire vec (although small)

```suggestion
    let (first, rest) = (all_edits.swap_remove(0), all_edits);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:346 on 2024-11-11 13:23_

The entire fix generation contains a fair amount of `unwrap` calls. Can we add comments explaining why they're "safe" or restructure the code in ways so that they aren't needed?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:308 on 2024-11-11 13:25_

I think we should just not provide a fix in this case or be `DisplayOnly`


```
    /// The fix is unsafe and should only be displayed for manual application by the user.
    /// The fix is likely to be incorrect or the resulting code may have invalid syntax.
    DisplayOnly,

    /// The fix is unsafe and should only be applied with user opt-in.
    /// The fix may be what the user intended, but it is uncertain; the resulting code will have valid syntax.
    Unsafe,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:367 on 2024-11-11 13:26_

Shouldn't we bail in this case too?

Can we add some tests demonstrating that this code path works as expected?



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:409 on 2024-11-11 13:27_

```suggestion
    let range = if index < last_index {
```

or should this include the last index? 

```
let range = if index <= last_index {

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:404 on 2024-11-11 13:30_

What are cases where the function has only one parameter but `first` isn't the parameter in question? Can we remove that check? If not, then add a test case for it and ensure the code below doesn't panic with only one parameter.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:406 on 2024-11-11 13:32_



```suggestion
    let typevar_range = parameters[index].range();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:297 on 2024-11-11 13:37_

We should bail if adding the import failed or we risk generating invalid code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:300 on 2024-11-11 13:40_

Are there cases where the parameter isn't guaranteed to be the first parameter? 

---

_@MichaReiser reviewed on 2024-11-11 13:42_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:404 on 2024-11-11 14:21_

Added a testcase:

```python
_S = TypeVar('_S', bound = 'C')

class C:
	def f[T](self: _S, a: T, b: T) -> _S: ...
```

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:346 on 2024-11-11 14:22_

One removed, two explained.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:300 on 2024-11-11 14:22_

Yes, see below.

---

_@InSyncWithFoo reviewed on 2024-11-11 14:22_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:409 on 2024-11-11 14:23_

Added two ASCII art comments.

---

_@InSyncWithFoo reviewed on 2024-11-11 14:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:325 on 2024-11-12 10:27_

```suggestion
    Fix::applicable_edits(first, rest, fix_applicability)
```

---

_@MichaReiser approved on 2024-11-12 10:28_

Nice thanks!

---

_Label `preview` added by @MichaReiser on 2024-11-12 10:28_

---

_Merged by @MichaReiser on 2024-11-12 11:13_

---

_Closed by @MichaReiser on 2024-11-12 11:13_

---

_Branch deleted on 2024-11-12 12:10_

---
