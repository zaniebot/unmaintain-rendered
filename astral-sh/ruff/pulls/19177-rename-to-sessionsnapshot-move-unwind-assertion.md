```yaml
number: 19177
title: "Rename to `SessionSnapshot`, move unwind assertion closer"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/address-review-comments
created_at: 2025-07-07T14:02:36Z
updated_at: 2025-07-07T14:14:25Z
url: https://github.com/astral-sh/ruff/pull/19177
synced_at: 2026-01-12T15:56:33Z
```

# Rename to `SessionSnapshot`, move unwind assertion closer

---

_@dhruvmanila_

This PR addresses the post-merge review comments from https://github.com/astral-sh/ruff/pull/19041, specifically it:
- Rename `WorkspaceSnapshot` to `SessionSnapshot`
- Rename `take_workspace_snapshot` to `take_session_snapshot`
- Rename `take_snapshot` to `take_document_snapshot`
- Move `AssertUnwindSafe` closer to the `catch_unwind` call which requires the assertion

---

_Review requested from @carljm by @dhruvmanila on 2025-07-07 14:02_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-07 14:02_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-07 14:02_

---

_Label `internal` added by @dhruvmanila on 2025-07-07 14:02_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-07 14:02_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-07 14:02_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-07 14:02_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-07 14:02_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-07 14:02_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-07 14:02_

---

_Comment by @github-actions[bot] on 2025-07-07 14:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
-     memo fields = ~49MB
+     memo fields = ~54MB

bidict (https://github.com/jab/bidict)
-     memo fields = ~17MB
+     memo fields = ~15MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

ignite (https://github.com/pytorch/ignite)
-     memo fields = ~189MB
+     memo fields = ~171MB

jinja (https://github.com/pallets/jinja)
-     memo fields = ~88MB
+     memo fields = ~80MB

pytest (https://github.com/pytest-dev/pytest)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

altair (https://github.com/vega/altair)
-     memo fields = ~251MB
+     memo fields = ~228MB

bokeh (https://github.com/bokeh/bokeh)
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~276MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

manticore (https://github.com/trailofbits/manticore)
-     memo fields = ~652MB
+     memo fields = ~593MB

```
</details>


---

_@MichaReiser approved on 2025-07-07 14:11_

Thank you

---

_Merged by @dhruvmanila on 2025-07-07 14:14_

---

_Closed by @dhruvmanila on 2025-07-07 14:14_

---

_Branch deleted on 2025-07-07 14:14_

---
