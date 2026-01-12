```yaml
number: 18321
title: "[ty] Infer types for ty_extensions.Intersection[A, B] tuple expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-366
created_at: 2025-05-26T14:59:09Z
updated_at: 2025-05-26T15:08:54Z
url: https://github.com/astral-sh/ruff/pull/18321
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Infer types for ty_extensions.Intersection[A, B] tuple expressions

---

_@sharkdp_

## Summary

fixes astral-sh/ty#366

## Test Plan

* Added panic corpus regression tests
* I also wrote a hover regression test (see below), but decided not to include it. The corpus tests are much more "effective" at finding these types of errors, since they exhaustively check all expressions for types.

<details>

```rs
#[test]
fn hover_regression_test_366() {
    let test = cursor_test(
        r#"
    from ty_extensions import Intersection

    class A: ...
    class B: ...

    def _(x: Intersection[A,<CURSOR> B]):
        pass
    "#,
    );

    assert_snapshot!(test.hover(), @r"
    A & B
    ---------------------------------------------
    ```text
    A & B
    ```
    ---------------------------------------------
    info[hover]: Hovered content is
     --> main.py:7:31
      |
    5 |         class B: ...
    6 |
    7 |         def _(x: Intersection[A, B]):
      |                               ^^-^
      |                               | |
      |                               | Cursor offset
      |                               source
    8 |             pass
      |
    ");
}
```

</details>

---

_Review requested from @carljm by @sharkdp on 2025-05-26 14:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-26 14:59_

---

_Review requested from @dcreager by @sharkdp on 2025-05-26 14:59_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-26 14:59_

---

_Label `ty` added by @sharkdp on 2025-05-26 14:59_

---

_Comment by @github-actions[bot] on 2025-05-26 15:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-26 15:02_

---

_Review comment by @sharkdp on `crates/ty_project/resources/test/corpus/ty_extensions.py`:21 on 2025-05-26 15:02_

The single-argument variant needs to be checked separately, as it takes a different code path. It's important *not* to store types for the single-argument variant, as that happens implicitly in the `self.infer_type_expression(element)` call a few lines above.

---

_@MichaReiser approved on 2025-05-26 15:08_

---

_Merged by @sharkdp on 2025-05-26 15:08_

---

_Closed by @sharkdp on 2025-05-26 15:08_

---

_Branch deleted on 2025-05-26 15:08_

---
