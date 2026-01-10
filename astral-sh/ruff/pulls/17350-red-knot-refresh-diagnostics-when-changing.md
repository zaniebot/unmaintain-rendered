```yaml
number: 17350
title: "[red-knot] Refresh diagnostics when changing related files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/inter-file-dependencies
created_at: 2025-04-11T12:47:10Z
updated_at: 2025-04-11T14:03:15Z
url: https://github.com/astral-sh/ruff/pull/17350
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Refresh diagnostics when changing related files

---

_Pull request opened by @MichaReiser on 2025-04-11 12:47_

## Summary

Given:

`main.py`:

```py
from foo import Foo
```

`foo.py`

```py
class Fo: ...
```

Red Knot reports an unresolved import diagnostic in `main.py` because `foo` doesn't export `Foo`. 
Renaming the class `Fo` in `foo.py` to `Foo` should refresh the diagnsotics in `main.py`.


It turns out, that this is a fairly easy change. All that was necessary is to set `inter_file_dependencies` in the server registration options to true.

We'll need a different solution for publish diagnostics but Red Knot doesn't yet support publish diagnostics (https://github.com/astral-sh/ty/issues/79)

## Test Plan


https://github.com/user-attachments/assets/7f54e675-919d-421d-b646-85fc96504ad2




---

_Label `server` added by @MichaReiser on 2025-04-11 12:47_

---

_Label `red-knot` added by @MichaReiser on 2025-04-11 12:47_

---

_Marked ready for review by @MichaReiser on 2025-04-11 12:47_

---

_Review requested from @carljm by @MichaReiser on 2025-04-11 12:47_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-11 12:47_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-11 12:47_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-11 12:47_

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-11 12:48_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-11 12:48_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-11 12:48_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-11 12:48_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-11 12:48_

---

_Comment by @github-actions[bot] on 2025-04-11 12:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[red-knot] Enable inter-file-dependencies" to "[red-knot] Refresh diagnostics when changing related files" by @MichaReiser on 2025-04-11 12:50_

---

_@dhruvmanila approved on 2025-04-11 13:49_

Nice!

---

_Merged by @MichaReiser on 2025-04-11 13:50_

---

_Closed by @MichaReiser on 2025-04-11 13:50_

---

_Branch deleted on 2025-04-11 13:50_

---

_Comment by @dhruvmanila on 2025-04-11 13:54_

> We'll need a different solution for publish diagnostics but Red Knot doesn't yet support publish diagnostics

Yeah, I think we might need to keep track of the diagnostic dependencies ourselves in that case and if any of the dependent file changes, we'll need to refresh the diagnostics for the parent files. This might also become useful and required for caching diagnostics on the server side.

---

_Comment by @MichaReiser on 2025-04-11 14:03_

> Yeah, I think we might need to keep track of the diagnostic dependencies ourselves in that case and if any of the dependent file changes, we'll need to refresh the diagnostics for the parent files. This might also become useful and required for caching diagnostics on the server side.

Yeah, I'm not sure how that would work because we have no way of asking: What files does file X depend on. Only including its direct dependencies isn't good enough. We might have to publish all diagnostics eagerly. I wanted to look into how r-a does that.

---
