```yaml
number: 19128
title: "[ty] don't allow first-party code to shadow stdlib types module"
type: pull_request
state: merged
author: carljm
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: cjm/noshadowtypes
created_at: 2025-07-03T17:44:43Z
updated_at: 2025-07-04T10:37:02Z
url: https://github.com/astral-sh/ruff/pull/19128
synced_at: 2026-01-10T18:33:12Z
```

# [ty] don't allow first-party code to shadow stdlib types module

---

_Pull request opened by @carljm on 2025-07-03 17:44_

## Summary

Fixes https://github.com/astral-sh/ty/issues/741

The situation with shadowing `types` at runtime is a bit muddy. It's not a builtin module, and isn't explicitly non-shadowable. But in practice, it is typically imported very early in interpreter startup, and this means in most cases it practically can't be shadowed. Even in the scenarios where shadowing it is possible, this usually just causes interpreter startup to fail, due to trying to import things from it.

Since shadowing it doesn't really work at runtime, and also causes problems in type-checking (since we try to import things from it implicitly), the simplest approach is just to not support shadowing it.

Neither mypy nor pyright support shadowing it. Mypy has a nice diagnostic specifically informing you that you are trying to shadow it and that doesn't work; I added a TODO for us to consider adding this in future.

## Test Plan

Added an mdtest which stack-overflowed before this PR.


---

_Label `ty` added by @carljm on 2025-07-03 17:44_

---

_Comment by @github-actions[bot] on 2025-07-03 17:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @github-actions[bot] on 2025-07-04 01:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~41MB
+     memo fields = ~45MB

dulwich (https://github.com/dulwich/dulwich)
-     memo fields = ~129MB
+     memo fields = ~117MB

discord.py (https://github.com/Rapptz/discord.py)
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~228MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~72MB
+     memo fields = ~66MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~189MB
+ TOTAL MEMORY USAGE: ~171MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~276MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1271MB
+ TOTAL MEMORY USAGE: ~1156MB

```
</details>


---

_Marked ready for review by @carljm on 2025-07-04 01:10_

---

_Review requested from @AlexWaygood by @carljm on 2025-07-04 01:10_

---

_Review requested from @sharkdp by @carljm on 2025-07-04 01:10_

---

_Review requested from @dcreager by @carljm on 2025-07-04 01:10_

---

_@sharkdp approved on 2025-07-04 06:42_

Thank you!

---

_@AlexWaygood approved on 2025-07-04 10:33_

Vanquisher of stack overflows!

---

_Label `bug` added by @AlexWaygood on 2025-07-04 10:34_

---

_Merged by @AlexWaygood on 2025-07-04 10:36_

---

_Closed by @AlexWaygood on 2025-07-04 10:36_

---

_Branch deleted on 2025-07-04 10:36_

---
