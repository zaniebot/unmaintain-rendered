```yaml
number: 11315
title: Add support for attribute docstring in the semantic model
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/attribute-docstring
created_at: 2024-05-07T03:13:48Z
updated_at: 2024-05-10T14:57:58Z
url: https://github.com/astral-sh/ruff/pull/11315
synced_at: 2026-01-12T15:55:37Z
```

# Add support for attribute docstring in the semantic model

---

_@dhruvmanila_

## Summary

This PR adds updates the semantic model to detect attribute docstring.

Refer to [PEP 258](https://peps.python.org/pep-0258/#attribute-docstrings) for the definition of an attribute docstring.

This PR doesn't add full support for it but only considers string literals as attribute docstring for the following cases:
1. A string literal following an assignment statement in the **global scope**.
2. A global class attribute

For an assignment statement, it's considered an attribute docstring only if the target expression is a name expression (`x = 1`). So, chained assignment, multiple assignment or unpacking, and starred expression, which are all valid in the target position, aren't considered here.

In `__init__` method, an assignment to the `self` variable like `self.x = 1` is also a candidate for an attribute docstring. **This PR does not support this position.**

## Test Plan

I used the following source code along with a print statement to verify that the attribute docstring detection is correct.

<details><summary>Source code to test against:</summary>
<p>

```py
a: int
"a: docstring"

b = 1
"b docstring" " continue"
"b: not a docstring"

c: int = 1
"c: docstring"

_a: int
"_a: docstring"

if True:
    d = 1
    "d: not a docstring"

(e := 1)
"e: not a docstring"

f = 0
f += 1
"f: not a docstring"

g.h = 1
"g.h: not a docstring"

(i) = 1
"i: docstring"

(j): int = 1
"j: docstring"

(k): int
"k: docstring"

l = m = 1
"l m: not a docstring"

n.a = n.b = n.c = 1
"n.*: not a docstring"

(o, p) = (1, 2)
"o p: not a docstring"

[q, r] = [1, 2]
"q r: not a docstring"

*s = 1
"s: not a docstring"


class Foo:
    a = 1
    "Foo.a: docstring"

    b: int
    "Foo.b: docstring"
    "Foo.b: not a docstring"

    c: int = 1
    "Foo.c: docstring"

    def __init__(self) -> None:
        self.x = 1
        "self.x: docstring"

        t = 2
        "t: not a docstring"

    def random(self):
        self.y = 2
        "self.y: not a docstring"

        u = 2
        "u: not a docstring"

    def add(self, y: int):
        self.x += y


def function():
    v = 2
    "v: not a docstring"


function.a = 1
"function.a: not a docstring"
```

</p>
</details> 

I'll add this in the follow-up PR (https://github.com/astral-sh/ruff/pull/11302) which uses this method.

---

_Label `internal` added by @dhruvmanila on 2024-05-07 03:15_

---

_Label `linter` added by @dhruvmanila on 2024-05-07 03:15_

---

_Comment by @github-actions[bot] on 2024-05-07 03:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-05-10 11:19_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-10 11:19_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-10 11:19_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 11:33_

Should the `in_docstring()` function immediately above this one have its name changed to `in_non_attribute_docstring()`?

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:2165 on 2024-05-10 11:35_

```suggestion
        /// ```
        ///
        /// Unlike other kinds of docstrings, attribute docstrings are discarded at runtime.
        /// However, they are used by some documentation renderers and static-analysis tools.
```

---

_@AlexWaygood approved on 2024-05-10 11:35_

---

_@dhruvmanila reviewed on 2024-05-10 12:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 12:09_

I'm not sure, I'd prefer to have a name which isn't a negation of the other 😅

---

_@AlexWaygood reviewed on 2024-05-10 12:11_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 12:11_

Yeah... `in_runtime_recognised_docstring()`? `in_runtime_stored_docstring()`?

---

_@dhruvmanila reviewed on 2024-05-10 12:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 12:19_

I guess `in_runtime_stored_docstring` could be used

---

_@AlexWaygood reviewed on 2024-05-10 12:23_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 12:23_

Or alternatively, rather than having two functions, we could have one `docstring_state()` method that returns an enum:

```rs
enum DocstringState {
    ClassOrModule,
    Attribute,
    None,
}
```

This might also better reflect the fact that it's invalid to have both the `ATTRIBUTE_DOCSTRING` and `DOCSTRING` flags simultaneously set. We could add a `debug_assert` in the `docstring_state()` method to enforce that invariant.

---

_@charliermarsh reviewed on 2024-05-10 13:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 13:19_

I would prefer `in_pep_257_docstring` and `in_attribute_docstring`.

---

_@dhruvmanila reviewed on 2024-05-10 14:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/model.rs`:1655 on 2024-05-10 14:40_

I'm not sure about the enum solution as it already exists to detect the docstring. For now, I'm thinking of going with the names suggested by Charlie.

Thank you for the suggestions!

---

_Label `linter` removed by @dhruvmanila on 2024-05-10 14:51_

---

_Merged by @dhruvmanila on 2024-05-10 14:57_

---

_Closed by @dhruvmanila on 2024-05-10 14:57_

---

_Branch deleted on 2024-05-10 14:57_

---
