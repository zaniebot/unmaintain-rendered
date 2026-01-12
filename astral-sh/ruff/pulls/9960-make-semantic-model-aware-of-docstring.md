```yaml
number: 9960
title: Make semantic model aware of docstring
type: pull_request
state: merged
author: dhruvmanila
labels:
  - docstring
assignees: []
merged: true
base: main
head: dhruv/semantic-model-docstring
created_at: 2024-02-12T19:09:57Z
updated_at: 2024-02-13T04:32:23Z
url: https://github.com/astral-sh/ruff/pull/9960
synced_at: 2026-01-12T15:55:30Z
```

# Make semantic model aware of docstring

---

_@dhruvmanila_

## Summary

This PR introduces a new semantic model flag `DOCSTRING` which suggests that the model is currently in a module / class / function docstring. This is the first step in eliminating the docstring detection state machine which is prone to bugs as stated in #7595.

## Test Plan

~TODO: Is there a way to add a test case for this?~

I tested this using the following code snippet and adding a print statement in the `string_like` analyzer to print if we're currently in a docstring or not.

<details><summary>Test code snippet:</summary>
<p>

```python
"Docstring" ", still a docstring"
"Not a docstring"


def foo():
    "Docstring"
    "Not a docstring"
    if foo:
        "Not a docstring"
        pass


class Foo:
    "Docstring"
    "Not a docstring"

    foo: int
    "Unofficial variable docstring"

    def method():
        "Docstring"
        "Not a docstring"
        pass


def bar():
    "Not a docstring".strip()


def baz():
    _something_else = 1
    """Not a docstring"""
```

</p>
</details> 


---

_Label `docstring` added by @dhruvmanila on 2024-02-12 19:09_

---

_Renamed from "Rename semantic model flag to MODULE_DOCSTRING_BOUNDARY" to "Make semantic model aware of docstring" by @dhruvmanila on 2024-02-12 19:10_

---

_Comment by @github-actions[bot] on 2024-02-12 19:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-12 21:27_

---

_@charliermarsh reviewed on 2024-02-13 00:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:393 on 2024-02-13 00:25_

I think we have an `is_docstring_stmt` helper in `ruff_python_ast` for this.

---

_@charliermarsh approved on 2024-02-13 00:25_

---

_Merged by @dhruvmanila on 2024-02-13 04:26_

---

_Closed by @dhruvmanila on 2024-02-13 04:26_

---

_Branch deleted on 2024-02-13 04:26_

---
