```yaml
number: 19994
title: "[ty] Fix namespace packages that behave like partial stubs"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: gankra/fix-cont
created_at: 2025-08-19T19:50:48Z
updated_at: 2025-08-20T06:03:16Z
url: https://github.com/astral-sh/ruff/pull/19994
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix namespace packages that behave like partial stubs

---

_Pull request opened by @Gankra on 2025-08-19 19:50_

In implementing partial stubs I had observed that this continue in the namespace package code seemed erroneous since the same continue for partial stubs didn't work. Unfortunately I wasn't confident enough to push on that hunch. Fortunately I remembered that hunch to make this an easy fix.

The issue with the continue is that it bails out of the current search-path without testing any .py files. This breaks when for example `google` and `google-stubs`/`types-google` are both in the same site-packages dir -- failing to find a module in `types-google` has us completely skip over `google`!

Fixes https://github.com/astral-sh/ty/issues/520

---

_Review requested from @carljm by @Gankra on 2025-08-19 19:50_

---

_Label `bug` added by @Gankra on 2025-08-19 19:50_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-19 19:50_

---

_Review requested from @sharkdp by @Gankra on 2025-08-19 19:50_

---

_Review requested from @dcreager by @Gankra on 2025-08-19 19:50_

---

_Label `ty` added by @Gankra on 2025-08-19 19:50_

---

_Comment by @github-actions[bot] on 2025-08-19 19:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 19:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@Gankra reviewed on 2025-08-19 19:56_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/partial_stub_packages.md`:59 on 2025-08-19 19:56_

(All these changes are a driveby fix that doesn't change the results but I realized was wrong and should be fixed)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/partial_stub_packages.md`:429 on 2025-08-19 20:19_

Do you have a reference that backs this claim? This is somewhat surprising to me and I remember that I struggled to find good documentation for what's supposed to happen in some of those non obvious cases

---

_@MichaReiser reviewed on 2025-08-19 20:19_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/partial_stub_packages.md`:429 on 2025-08-19 20:26_

https://peps.python.org/pep-0561/#partial-stub-packages

> Type checkers should treat namespace packages within stub-packages as incomplete since multiple distributions may populate them. Regular packages within namespace packages in stub-package distributions are considered complete unless a py.typed with partial\n is included.

---

_@Gankra reviewed on 2025-08-19 20:26_

---

_@carljm approved on 2025-08-19 20:33_

Looks good, thank you!

---

_Merged by @Gankra on 2025-08-19 20:34_

---

_Closed by @Gankra on 2025-08-19 20:34_

---

_Branch deleted on 2025-08-19 20:34_

---

_@MichaReiser reviewed on 2025-08-20 06:03_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/partial_stub_packages.md`:429 on 2025-08-20 06:03_

It would be great to add this link to the test

---
