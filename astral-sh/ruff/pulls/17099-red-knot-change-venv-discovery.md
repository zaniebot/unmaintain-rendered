```yaml
number: 17099
title: "[red-knot] Change venv discovery"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/venv-relative-to-project
created_at: 2025-03-31T17:05:39Z
updated_at: 2025-03-31T17:39:07Z
url: https://github.com/astral-sh/ruff/pull/17099
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Change venv discovery

---

_@MichaReiser_

## Summary

Rewrites the virtual env discovery to:

* Only use of `System` APIs, this ensures that the discovery will also work when using a memory file system (testing or WASM)
* Don't traverse ancestor directories. We're not convinced that this is necessary. Let's wait until someone shows us a use case where it is needed
* Start from the project root and not from the current working directory. This ensures that Red Knot picks up the right venv even when using `knot --project ../other-dir` 

## Test Plan

Existing tests, @ntBre tested that the `file_watching` tests no longer pick up his virtual env in a parent directory


---

_Review requested from @carljm by @MichaReiser on 2025-03-31 17:05_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-31 17:05_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-31 17:05_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-31 17:05_

---

_Label `red-knot` added by @MichaReiser on 2025-03-31 17:05_

---

_Comment by @github-actions[bot] on 2025-03-31 17:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @ntBre on 2025-03-31 17:17_

I can confirm that all tests pass on this branch, while `remove_search_path` fails on the parent commit (4a6fa5fc2).

---

_@AlexWaygood approved on 2025-03-31 17:23_

---

_Comment by @MichaReiser on 2025-03-31 17:38_

We talked internally about if we should keep the ancestor traversal. We'll revisit this later and re-add the logic if we think it's necessary (CC: @zanieb)

---

_Merged by @MichaReiser on 2025-03-31 17:39_

---

_Closed by @MichaReiser on 2025-03-31 17:39_

---

_Branch deleted on 2025-03-31 17:39_

---
