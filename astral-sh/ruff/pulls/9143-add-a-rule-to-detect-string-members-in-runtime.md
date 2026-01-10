```yaml
number: 9143
title: Add a rule to detect string members in runtime-evaluated unions
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/type
created_at: 2023-12-15T01:37:21Z
updated_at: 2023-12-16T21:28:51Z
url: https://github.com/astral-sh/ruff/pull/9143
synced_at: 2026-01-10T23:31:11Z
```

# Add a rule to detect string members in runtime-evaluated unions

---

_Pull request opened by @charliermarsh on 2023-12-15 01:37_

## Summary

A common mistake is to add quotes around one member in an `X | Y`-style type union, as in:

```python
contract_versions_list: list[ContractVersion] | 'QuerySet[ContractVersion]' | None = None
```

However, doing so will lead to a runtime error if the annotation is runtime-evaluated. This PR lints against such patterns.

Closes https://github.com/astral-sh/ruff/issues/9139.


---

_Label `rule` added by @charliermarsh on 2023-12-15 01:37_

---

_Label `preview` added by @charliermarsh on 2023-12-15 01:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:32 on 2023-12-15 01:50_

Would `var: "str | int"` be valid too?

---

_Comment by @github-actions[bot] on 2023-12-15 01:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @zanieb by @charliermarsh on 2023-12-15 01:52_

---

_Review requested from @konstin by @charliermarsh on 2023-12-15 01:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:76 on 2023-12-15 01:53_

Does python support indexer types similar to typescript that need traversing?

```
type A = Value[int | string]
```

or is it safe to assume that union types are always top-level?

Edit: I think this is a valid example for when the union is not top level: 

```python
callback: Callable[[str], Awaitable[None | str | "int"]] = on_update
```

or 

```python
# Type checker will infer that all keys in ``z`` are meant to be strings,
# and that all values in ``z`` are meant to be either strings or ints
z: Mapping[str, str | int] = {}
```

Some other examples where I'm not entirely sure if they're valid/we need to handle here

```python
OldS = TypeVar('OldS', int | 'str', str)
```

```python
def inproduct[T: (int | complex, float, complex)](v: Vec[T]) -> T: # Same as Iterable[tuple[T, T]]
    return sum(x*y for x, y in v)
```

Do we need to handle `Annotated`?

---

_@MichaReiser reviewed on 2023-12-15 01:58_

---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH006.py`:4 on 2023-12-15 09:53_

Could you add a test case with `from __future__ import annotations`?

---

_@konstin approved on 2023-12-15 10:06_

---

_@charliermarsh reviewed on 2023-12-16 15:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:32 on 2023-12-16 15:50_

Yes!

---

_@charliermarsh reviewed on 2023-12-16 15:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:76 on 2023-12-16 15:51_

Yup these are all valid and we should already be handling them correctly outside of the specific context of this rule, since these traversals are encoded in the `Semantic` model (i.e., whether we're in a type definition, and whether it will be evaluated at runtime).

---

_@charliermarsh reviewed on 2023-12-16 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:76 on 2023-12-16 21:14_

Added more tests, thanks :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TCH006.py`:4 on 2023-12-16 21:16_

Done!

---

_@charliermarsh reviewed on 2023-12-16 21:16_

---

_Merged by @charliermarsh on 2023-12-16 21:22_

---

_Closed by @charliermarsh on 2023-12-16 21:22_

---

_Branch deleted on 2023-12-16 21:22_

---
