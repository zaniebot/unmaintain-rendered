```yaml
number: 1488
title: "`available_submodule_attributes` should probably be considered last instead of first"
type: issue
state: open
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:38:51Z
updated_at: 2025-11-11T21:20:14Z
url: https://github.com/astral-sh/ty/issues/1488
synced_at: 2026-01-10T02:06:25Z
```

# `available_submodule_attributes` should probably be considered last instead of first

---

_Issue opened by @Gankra on 2025-11-05 17:38_

See here for background on `available_submodule_attributes`:

https://github.com/astral-sh/ruff/blob/cef6600cf3a7a22db214c6cb5d2393ede4209c37/crates/ty_python_semantic/src/types.rs#L10997-L11060

As discussed there, `available_submodule_attributes` is dangerously high priority and early in our analysis right now ("wins all tie-breaks"). In  https://github.com/astral-sh/ruff/pull/21173 I actually rolled back my own attempts to expand the functionality of it, after we determined it was too semantically limited and heavy of a hammer for the problem we wanted to solve.

That said, `available_submodule_attributes` still has value, as this is basically the only way to introduce sub-attributes on modules other than the current file (i.e. to encode the effect that `import a.b.c` should make `a.b` and `a.b.c` available in-scope).

I believe we would be able to expand its functionality if we inverted its priority: instead of being the *first* thing we check, it should be the *last* thing we check. That is, if a module otherwise defines a local with that name, it should shadow the submodule attribute. This would make it more ok to eagerly shove as much as possible into `available_submodule_attributes`.

(I'm filling a couple issues that IMO "need" this as a prerequisite.) 

---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:38_

---

_Label `imports` added by @Gankra on 2025-11-05 17:38_

---

_Unassigned @Gankra by @Gankra on 2025-11-11 21:20_

---
