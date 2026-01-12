```yaml
number: 1867
title: "Improve `SIM117`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: improve-SIM117
created_at: 2023-01-14T07:50:53Z
updated_at: 2023-01-14T12:59:25Z
url: https://github.com/astral-sh/ruff/pull/1867
synced_at: 2026-01-12T15:55:07Z
```

# Improve `SIM117`

---

_@harupy_

This PR makes the following changes to improve `SIM117`:

- Avoid emitting `SIM117` multiple times within the same `with` statement:
- Adjust the error range.  


## Example

```python
with A() as a:  # SIM117
    with B() as b:
        with C() as c:
            print("hello")
```

### Current

```
resources/test/fixtures/flake8_simplify/SIM117.py:5:1: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
  |
5 | / with A() as a:  # SIM117
6 | |     with B() as b:
7 | |         with C() as c:
8 | |             print("hello")
  | |__________________________^ SIM117
  |

resources/test/fixtures/flake8_simplify/SIM117.py:6:5: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
  |
6 |       with B() as b:
  |  _____^
7 | |         with C() as c:
8 | |             print("hello")
  | |__________________________^ SIM117
  |
```

### Improved

```
resources/test/fixtures/flake8_simplify/SIM117.py:5:1: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
  |
5 | / with A() as a:  # SIM117
6 | |     with B() as b:
7 | |         with C() as c:
  | |______________________^ SIM117
  |
```

---

_Review comment by @not-my-profile on `src/flake8_simplify/rules/ast_with.rs`:9 on 2023-01-14 08:40_

I think the return type here could be precised, since it will always return a `StmtKind::With` ... since rustpython_ast unfortunately doesn't define a struct for the fields of the `StmtKind::With` variant we'd have to define that struct ourselves ... but I think it would still be worth it for the additional clarity ... and then we could remove the weird looking `else { return }` in the `multiple_with_statements` function.

---

_@not-my-profile reviewed on 2023-01-14 08:40_

---

_@not-my-profile reviewed on 2023-01-14 10:01_

---

_Review comment by @not-my-profile on `src/flake8_simplify/rules/ast_with.rs`:16 on 2023-01-14 10:01_

I think the following would be more idiomatic:

```rust
    let [StmtKind::With { items, body, .. }] = body else { return; };
```

---

_@not-my-profile reviewed on 2023-01-14 10:03_

---

_Review comment by @not-my-profile on `src/ast/helpers.rs`:625 on 2023-01-14 10:03_

Doc comments should start with three slashes (`///`) and `Tok::Colon` and `Range` can be wrapped in [] so that rustdoc turns these references into links (you can check that with `cargo doc --no-deps --open`).

---

_Review comment by @harupy on `src/ast/helpers.rs`:625 on 2023-01-14 10:05_

Thanks for the catch!

---

_@harupy reviewed on 2023-01-14 10:05_

---

_@not-my-profile reviewed on 2023-01-14 10:06_

---

_Review comment by @not-my-profile on `src/flake8_simplify/rules/ast_with.rs`:25 on 2023-01-14 10:06_

I think this signature has gotten complex enough to warrant a doc comment that explains what these various `Stmt`s should be. Or perhaps the parameter names could be clarified ... just `stmt`, `body` and `parent` aren't really clear: what kind of statement? body of what? parent of what?

---

_Review comment by @harupy on `src/flake8_simplify/rules/ast_with.rs`:25 on 2023-01-14 10:10_

can we rename variable names?

---

_@harupy reviewed on 2023-01-14 10:10_

---

_Review comment by @harupy on `src/flake8_simplify/rules/ast_with.rs`:16 on 2023-01-14 10:11_

```rust
    let [StmtKind::With { items, body, .. }] = body else { return; };
```

Does this really work? `body` is `&[Stmt]`, not `&[StmtKind]`?

---

_@harupy reviewed on 2023-01-14 10:11_

---

_@harupy reviewed on 2023-01-14 10:13_

---

_Review comment by @harupy on `src/flake8_simplify/rules/ast_with.rs`:25 on 2023-01-14 10:13_

Renamed the arguments.

---

_@harupy reviewed on 2023-01-14 10:20_

---

_Review comment by @harupy on `src/flake8_simplify/rules/ast_with.rs`:16 on 2023-01-14 10:20_

I think we could do:

```diff
 fn find_last_with(body: &[Stmt]) -> Option<(&Vec<Withitem>, &Vec<Stmt>)> {
-    if body.len() != 1 {
+    let [stmt] = body else {
         return None;
-    }
-    let StmtKind::With { items, body, .. } = &body[0].node else {
+    };
+    let StmtKind::With { items, body, .. } = &stmt.node else {
         return None
     };
     find_last_with(body).or(Some((items, body)))
     find_last_with(body).or(Some((items, body)))
```

but not sure if it't worth it

---

_Review requested from @not-my-profile by @harupy on 2023-01-14 10:20_

---

_@not-my-profile reviewed on 2023-01-14 10:25_

---

_Review comment by @not-my-profile on `src/flake8_simplify/rules/ast_with.rs`:16 on 2023-01-14 10:25_

Ah right I missed the additional `Locate` nesting ... I think sth like the following should work:
```rs
    let [Locate { node: StmtKind::With { items, body, .. }, ..}] = body else { return; };
```

---

_Review comment by @harupy on `src/ast/helpers.rs`:625 on 2023-01-14 10:31_

> Tok::Colon and Range can be wrapped in [] so that rustdoc turns these references into links (you can check that with cargo doc --no-deps --open).


I didn't know that. So if we wrap `Tok::Colon` and `Range` in `[]`, does rustdoc create links to RustPython's doc?

---

_@harupy reviewed on 2023-01-14 10:31_

---

_Merged by @charliermarsh on 2023-01-14 12:59_

---

_Closed by @charliermarsh on 2023-01-14 12:59_

---
