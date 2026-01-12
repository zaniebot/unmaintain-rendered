```yaml
number: 21119
title: Update stdlibs with 3.15
type: issue
state: open
author: amyreese
labels:
  - upstream
  - python315
assignees: []
created_at: 2025-10-29T03:08:23Z
updated_at: 2025-10-29T17:54:00Z
url: https://github.com/astral-sh/ruff/issues/21119
synced_at: 2026-01-12T15:54:57Z
```

# Update stdlibs with 3.15

---

_@amyreese_

Not sure when we normally do this, but at some point we'll need to update the known stdlib list, and I just pushed a `stdlibs` release earlier today with a list of modules from 3.15.0a1.

---

_Assigned to @amyreese by @amyreese on 2025-10-29 03:08_

---

_Label `upstream` added by @amyreese on 2025-10-29 03:08_

---

_Comment by @MichaReiser on 2025-10-29 14:52_

We do have a versioned list of all builtins in ruff (see [builtins.rs](https://github.com/astral-sh/ruff/blob/c4506f0e94882fa1118eb2a13339f9be7bf0bcaa/crates/ruff_python_stdlib/src/builtins.rs#L14)). I'm not aware of any place where we check for builtin modules, but I might be mistaken. Do you have a reference where Ruff checks for built-in modules?

---

_Comment by @amyreese on 2025-10-29 16:31_

It's part of isort, so that it knows which modules are first-party/stdlib for sorting into categories.

---

_Comment by @MichaReiser on 2025-10-29 17:53_

Oh this one https://github.com/astral-sh/ruff/blob/bc7b30364dcd196f9d660218b852870b1d978e76/crates/ruff_python_stdlib/src/sys/known_stdlib.rs#L1-L0

I think we can update it anytime, as the imports are by Python version. We might just need it to do multiple times. The first step is to add Python 3.15 to the supported python versions

---

_Label `python315` added by @MichaReiser on 2025-10-29 17:54_

---
