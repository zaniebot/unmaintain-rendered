```yaml
number: 16273
title: "red_knot_python_semantic: avoid adding `callable_ty` to `CallBindingError`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/callable-ty-tweak
created_at: 2025-02-20T13:08:20Z
updated_at: 2025-02-20T13:19:02Z
url: https://github.com/astral-sh/ruff/pull/16273
synced_at: 2026-01-12T15:55:54Z
```

# red_knot_python_semantic: avoid adding `callable_ty` to `CallBindingError`

---

_@BurntSushi_

This is a small tweak to avoid adding the callable `Type` on the error
value itself. Namely, it's always available regardless of the error, and
it's easy to pass it down explicitly to the diagnostic generating code.

It's likely that the other `CallBindingError` variants will also want
the callable `Type` to improve diagnostics too. This way, we don't have
to duplicate the `Type` on each variant. It's just available to all of
them.

Ref https://github.com/astral-sh/ruff/pull/16239#discussion_r1962352646


---

_Review requested from @carljm by @BurntSushi on 2025-02-20 13:08_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-20 13:08_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-20 13:08_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-20 13:08_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-20 13:11_

---

_@AlexWaygood approved on 2025-02-20 13:17_

---

_Merged by @BurntSushi on 2025-02-20 13:18_

---

_Closed by @BurntSushi on 2025-02-20 13:18_

---

_Branch deleted on 2025-02-20 13:19_

---
