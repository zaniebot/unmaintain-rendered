```yaml
number: 19186
title: "[ty] Add a new property test: all types assignable to `Iterable[object]` should be considered iterable"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/iterability-property-test
created_at: 2025-07-07T15:44:44Z
updated_at: 2025-07-08T09:54:09Z
url: https://github.com/astral-sh/ruff/pull/19186
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Add a new property test: all types assignable to `Iterable[object]` should be considered iterable

---

_@AlexWaygood_

## Summary

Any type assignable to `Iterable[object]` should also be considered iterable by ty. Currently we consider far too many types to be assignable to `Iterable[object]`; this is one of the causes of the crash in https://github.com/astral-sh/ty/issues/764. As such, the property test added here is added as a flaky property test for now; the tests find many types for which this invariant currently does not hold true.

Note that there are many objects which are not assignable to `Iterable[object]`, but which we nonetheless consider iterable. This is a deliberate feature: as well as understanding the new-style iteration protocol (represented by `Iterable`), we also understand the old-style iteration protocol that works via the `__getitem__` dunder.

## Test Plan

`cargo test --release -p ty_python_semantic -- --ignored types::property_tests::flaky`


---

_Review requested from @carljm by @AlexWaygood on 2025-07-07 15:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-07 15:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-07 15:44_

---

_Label `testing` added by @AlexWaygood on 2025-07-07 15:44_

---

_Label `ty` added by @AlexWaygood on 2025-07-07 15:44_

---

_Comment by @github-actions[bot] on 2025-07-07 15:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
-     memo fields = ~17MB
+     memo fields = ~15MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

rich (https://github.com/Textualize/rich)
-     memo fields = ~117MB
+     memo fields = ~106MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

porcupine (https://github.com/Akuli/porcupine)
-     memo fields = ~97MB
+     memo fields = ~106MB

ignite (https://github.com/pytorch/ignite)
-     memo fields = ~171MB
+     memo fields = ~189MB

jinja (https://github.com/pallets/jinja)
-     memo fields = ~80MB
+     memo fields = ~88MB

colour (https://github.com/colour-science/colour)
- TOTAL MEMORY USAGE: ~445MB
+ TOTAL MEMORY USAGE: ~405MB

altair (https://github.com/vega/altair)
-     memo fields = ~251MB
+     memo fields = ~228MB

bokeh (https://github.com/bokeh/bokeh)
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~276MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1399MB
+ TOTAL MEMORY USAGE: ~1271MB

```
</details>


---

_@sharkdp approved on 2025-07-08 08:52_

Nice

---

_Merged by @AlexWaygood on 2025-07-08 09:54_

---

_Closed by @AlexWaygood on 2025-07-08 09:54_

---

_Branch deleted on 2025-07-08 09:54_

---
