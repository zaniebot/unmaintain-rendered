```yaml
number: 19202
title: "[ty] Add tests for dataclass fields annotated with `Final`"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/dataclass-final-fields
created_at: 2025-07-08T11:26:42Z
updated_at: 2025-07-08T12:34:26Z
url: https://github.com/astral-sh/ruff/pull/19202
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Add tests for dataclass fields annotated with `Final`

---

_Pull request opened by @sharkdp on 2025-07-08 11:26_

## Summary

Adds some tests for dataclass fields that are annotated with `Final` (see comment [here](https://github.com/astral-sh/ruff/pull/15768#issuecomment-3044737645)). Turns out that nothing is needed here, everything already works as expected (apart from the fact that we can assign to `Final` fields, which is tracked in https://github.com/astral-sh/ty/issues/158

## Test Plan

New Markdown tests

---

_Review requested from @carljm by @sharkdp on 2025-07-08 11:26_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-08 11:26_

---

_Review requested from @dcreager by @sharkdp on 2025-07-08 11:26_

---

_Label `ty` added by @sharkdp on 2025-07-08 11:26_

---

_Comment by @github-actions[bot] on 2025-07-08 11:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:461 on 2025-07-08 11:51_

I think this is missing a test for the most important difference here between the way dataclasses work and the way other classes work, which is that -- unlike normal classes -- this should be allowed, even though there is no assignment to the variable in the class body:

```py
@dataclass
class C:
    foo: Final[int]
```

That's because this class conceptually desugars to this:

```py
class C:
    def __init__(self, foo: int) -> None:
        self.x: Final[int] = foo
```

Which we'd obviously allow.

There's a test for this in the typing spec conformance suite [here](https://github.com/python/typing/blob/main/conformance/tests/dataclasses_final.py), and pyright [implements the behaviour correctly](https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQokUAJsTMQManEDOrApq5trvnQ2ZtW5cgAEBTFu3JD2hMGABc5KGqgAPJYRKkA2qhgBdUcCgBeBWAAUAFgBMASnIgOANw5kA%2BvAQdrwAB0Gs5AA), although [mypy does not](https://mypy-play.net/?mypy=latest&python=3.12&gist=22998ae216fe426604d50e5ece691968)

---

_@AlexWaygood approved on 2025-07-08 11:51_

---

_Label `testing` added by @AlexWaygood on 2025-07-08 11:52_

---

_@sharkdp reviewed on 2025-07-08 12:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:461 on 2025-07-08 12:25_

> I think this is missing a test for the most important difference here between the way dataclasses work and the way other classes work

Yes, thank you! Added tests for dataclasses and normal classes.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:462 on 2025-07-08 12:27_

```suggestion
    instance_variable_no_default: Final[int]  # not having a RHS for a `Final` declaration is disallowed for non-dataclasses but allowed for dataclasses
```

---

_@AlexWaygood approved on 2025-07-08 12:28_

---

_Merged by @sharkdp on 2025-07-08 12:33_

---

_Closed by @sharkdp on 2025-07-08 12:33_

---

_Branch deleted on 2025-07-08 12:33_

---
