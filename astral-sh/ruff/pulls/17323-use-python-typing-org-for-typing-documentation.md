```yaml
number: 17323
title: Use python.typing.org for typing documentation links
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: david/typing.python.org
created_at: 2025-04-09T18:31:32Z
updated_at: 2025-04-09T18:38:24Z
url: https://github.com/astral-sh/ruff/pull/17323
synced_at: 2026-01-12T15:56:01Z
```

# Use python.typing.org for typing documentation links

---

_@sharkdp_

## Summary

There is a new official URL for the typing documentation: https://typing.python.org/

Change all https://typing.readthedocs.io/ links to use the new sub domain, which is slightly shorter and looks more official.

## Test Plan

Tested to see if each and every new URL is accessible. I noticed that some links go to https://typing.python.org/en/latest/source/stubs.html which seems to be outdated, but that is a separate issue. The same page shows up for the old URL.

---

_Label `documentation` added by @sharkdp on 2025-04-09 18:31_

---

_Label `internal` added by @sharkdp on 2025-04-09 18:31_

---

_Review requested from @carljm by @sharkdp on 2025-04-09 18:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-09 18:31_

---

_Review requested from @dcreager by @sharkdp on 2025-04-09 18:31_

---

_Review requested from @MichaReiser by @sharkdp on 2025-04-09 18:31_

---

_Comment by @github-actions[bot] on 2025-04-09 18:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-04-09 18:37_

---

_Merged by @sharkdp on 2025-04-09 18:38_

---

_Closed by @sharkdp on 2025-04-09 18:38_

---

_Comment by @github-actions[bot] on 2025-04-09 18:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-04-09 18:38_

---
