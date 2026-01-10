```yaml
number: 16130
title: "(🐞) `subclass-builtin` (`FURB189`) doesn't apply when type arguments are supplied"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-13T00:18:21Z
updated_at: 2025-02-14T19:21:28Z
url: https://github.com/astral-sh/ruff/issues/16130
synced_at: 2026-01-10T11:09:57Z
```

# (🐞) `subclass-builtin` (`FURB189`) doesn't apply when type arguments are supplied

---

_Issue opened by @KotlinIsland on 2025-02-13 00:18_

### Description

```py
class A(dict): ...  #  Subclassing `dict` can be error prone, use `collections.UserDict` instead


class B(dict[str, str]): ...  # no error
```

---

_Comment by @InSyncWithFoo on 2025-02-13 03:27_

This would be a good first issue. The logic around [here](https://github.com/astral-sh/ruff/blob/f8093b65ea88eb11a0d0f4ffdbf7c6f007043238/crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs#L73-L83) needs a few modifications to also account for subscript expressions.





---

_Label `bug` added by @MichaReiser on 2025-02-13 08:07_

---

_Label `help wanted` added by @MichaReiser on 2025-02-13 08:07_

---

_Comment by @vladNed on 2025-02-14 06:12_

Is there anyone else already working on this or can I give it a go ? 

---

_Comment by @MichaReiser on 2025-02-14 07:04_

I don't think so. Sure, go for it

---

_Assigned to @vladNed by @MichaReiser on 2025-02-14 07:04_

---

_Closed by @MichaReiser on 2025-02-14 19:21_

---

_Closed by @MichaReiser on 2025-02-14 19:21_

---
