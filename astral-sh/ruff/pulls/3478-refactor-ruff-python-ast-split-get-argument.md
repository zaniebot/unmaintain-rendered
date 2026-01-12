```yaml
number: 3478
title: "refactor(ruff_python_ast): Split `get_argument`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/split-get-argument
created_at: 2023-03-13T08:44:35Z
updated_at: 2023-03-13T17:18:27Z
url: https://github.com/astral-sh/ruff/pull/3478
synced_at: 2026-01-12T04:39:44Z
```

# refactor(ruff_python_ast): Split `get_argument`

---

_Pull request opened by @MichaReiser on 2023-03-13 08:44_

This PR splits `SimpleCallArguments::get_arguments` into two methods:

* `keyword_argument`: Retrieves an argument by name only
* `argument`: Retrieves an argument by name or position

Splitting the methods has the advantage that:

* It's no longer necessary to pass `None` (or wrap the position in `Some`) when retrieving a keyword argument that has no position
* We hint to the compiler that checking if `position` is `Some` is never necessary for keyword arguments.
* The method names use the python semantics of keyword arguments vs "regular arguments"

---

_Label `internal` added by @MichaReiser on 2023-03-13 08:44_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-13 08:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/helpers.rs`:1220 on 2023-03-13 08:45_

Using `collect` is potentially faster than adding element by element because `collect` can use `Iterator::size_hint` to *guess* the necessary capacity.

---

_@MichaReiser reviewed on 2023-03-13 08:45_

---

_@MichaReiser reviewed on 2023-03-13 08:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/helpers.rs`:1248 on 2023-03-13 08:47_

Does it matter if we first retrieve the argument by name or position? If it doesn't, then first retrieving the argument by position could speedup the linter because a vec position lookup is significantly faster than a hashmap lookup. 

All tests continue to pass if I flip the lookup order. 

---

_Comment by @github-actions[bot] on 2023-03-13 09:01_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@charliermarsh approved on 2023-03-13 12:51_

---

_Merged by @MichaReiser on 2023-03-13 17:18_

---

_Closed by @MichaReiser on 2023-03-13 17:18_

---

_Branch deleted on 2023-03-13 17:18_

---
