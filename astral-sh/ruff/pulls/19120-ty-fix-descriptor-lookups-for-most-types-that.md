```yaml
number: 19120
title: "[ty] Fix descriptor lookups for most types that overlap with `None`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/none-descr
created_at: 2025-07-03T13:22:57Z
updated_at: 2025-07-07T06:38:52Z
url: https://github.com/astral-sh/ruff/pull/19120
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Fix descriptor lookups for most types that overlap with `None`

---

_Pull request opened by @AlexWaygood on 2025-07-03 13:22_

## Summary

Helps with https://github.com/astral-sh/ty/issues/737. The problem remains for calling methods on `None` itself, but no longer applies to `object`, `AlwaysTruthy`, `AlwaysFalsy`, or protocols that are not disjoint from `None`.

## Test Plan

mdtests


---

_Label `ty` added by @AlexWaygood on 2025-07-03 13:23_

---

_Comment by @github-actions[bot] on 2025-07-03 13:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
-     memo fields = ~54MB
+     memo fields = ~49MB

async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

werkzeug (https://github.com/pallets/werkzeug)
- error[non-subscriptable] src/werkzeug/utils.py:91:13: Cannot subscript object of type `property` with no `__getitem__` method
+ error[non-subscriptable] src/werkzeug/utils.py:91:13: Cannot subscript object of type `object` with no `__getitem__` method
- error[non-subscriptable] src/werkzeug/utils.py:118:17: Cannot subscript object of type `property` with no `__getitem__` method
+ error[non-subscriptable] src/werkzeug/utils.py:118:17: Cannot subscript object of type `object` with no `__getitem__` method

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

rich (https://github.com/Textualize/rich)
-     memo fields = ~117MB
+     memo fields = ~106MB

isort (https://github.com/pycqa/isort)
-     memo metadata = ~5MB
+     memo metadata = ~6MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~49MB
+     memo fields = ~54MB

poetry (https://github.com/python-poetry/poetry)
-     memo fields = ~189MB
+     memo fields = ~171MB

jinja (https://github.com/pallets/jinja)
-     memo fields = ~80MB
+     memo fields = ~88MB

pytest (https://github.com/pytest-dev/pytest)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

vision (https://github.com/pytorch/vision)
- error[missing-argument] torchvision/datasets/vision.py:81:17: No argument provided for required parameter `self` of function `__repr__`
- error[missing-argument] torchvision/datasets/vision.py:101:17: No argument provided for required parameter `self` of function `__repr__`
- Found 1474 diagnostics
+ Found 1472 diagnostics

psycopg (https://github.com/psycopg/psycopg)
-     memo metadata = ~25MB
+     memo metadata = ~28MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

bokeh (https://github.com/bokeh/bokeh)
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~276MB

prefect (https://github.com/PrefectHQ/prefect)
- error[missing-argument] src/prefect/settings/legacy.py:80:16: No argument provided for required parameter `value` of function `__eq__`
- Found 3753 diagnostics
+ Found 3752 diagnostics

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1399MB
+ TOTAL MEMORY USAGE: ~1271MB

```
</details>


---

_Marked ready for review by @AlexWaygood on 2025-07-03 13:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-03 13:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-03 13:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-03 13:28_

---

_@AlexWaygood reviewed on 2025-07-03 13:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-07-03 13:33_

this means that we now selected overload 2 rather than overload 1 for types such as `object` that are not subtypes of `None`, but are also not disjoint from `None`

---

_Label `bug` added by @AlexWaygood on 2025-07-03 13:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:624 on 2025-07-03 17:12_

I think we should preserve a TODO here

---

_@carljm approved on 2025-07-03 17:13_

---

_@sharkdp reviewed on 2025-07-03 17:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3578 on 2025-07-03 17:45_

I think you may want to apply the same change to the match case just above, right? It's slightly harder to write a test for, because you might need to call `__get__` manually? Let me know if you need help.

---

_@AlexWaygood reviewed on 2025-07-05 18:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3578 on 2025-07-05 18:30_

thanks, yes, you're correct that we emitted false positive diagnostics for `object.__str__.__get__(object(), None)()` as well as `object().__str__()` 😆

I added a regression test!

---

_Merged by @AlexWaygood on 2025-07-05 18:34_

---

_Closed by @AlexWaygood on 2025-07-05 18:34_

---

_Branch deleted on 2025-07-05 18:34_

---

_@sharkdp reviewed on 2025-07-07 06:38_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3578 on 2025-07-07 06:38_

> `object.__str__.__get__(object(), None)()`

Just imagine how happy users will be that this *finally* works correctly.

---
