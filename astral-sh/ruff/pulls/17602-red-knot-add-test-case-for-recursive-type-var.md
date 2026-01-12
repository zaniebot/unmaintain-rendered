```yaml
number: 17602
title: "[red-knot] Add test case for recursive type var constraint"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/tracked-read-on-struct-being-initialized
created_at: 2025-04-24T10:01:40Z
updated_at: 2025-05-29T08:56:42Z
url: https://github.com/astral-sh/ruff/pull/17602
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Add test case for recursive type var constraint

---

_@MichaReiser_

Failing test for https://github.com/astral-sh/ruff/issues/17600

---

_Label `red-knot` added by @AlexWaygood on 2025-04-24 10:02_

---

_Comment by @MichaReiser on 2025-04-25 17:21_

I narrowed this to a salsa bug. Closing in favor of the salsa test case

---

_Closed by @MichaReiser on 2025-04-25 17:21_

---

_Reopened by @MichaReiser on 2025-04-28 11:13_

---

_Comment by @MichaReiser on 2025-04-28 11:14_

@carljm or @dcreager  this now fails with `crates/red_knot_python_semantic/resources/mdtest/cycle_panic.md:10 dependency graph cycle querying generic_context_(Id(1000)); set cycle_fn/cycle_initial to fixpoint iterate` after rebasing onto main. Could either of you take this on as this is no longer a salsa bug


---

_Renamed from "[red-knot] Add test case for read on tracked struct being initialized" to "[red-knot] Add test case for recursive type var constraint" by @MichaReiser on 2025-04-28 11:14_

---

_Comment by @carljm on 2025-04-28 23:14_

I can look at this.

---

_Comment by @MichaReiser on 2025-05-26 12:16_

@dcreager / @carljm Feel free to close this PR if the issue has been addressed 

---

_Comment by @carljm on 2025-05-27 18:23_

It's not fixed, but I added the example case to https://github.com/astral-sh/ty/issues/256, so we can close this PR.

---

_Closed by @carljm on 2025-05-27 18:23_

---
