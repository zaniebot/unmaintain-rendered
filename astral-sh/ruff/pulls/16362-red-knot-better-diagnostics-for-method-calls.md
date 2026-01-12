```yaml
number: 16362
title: "[red-knot] Better diagnostics for method calls"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/diagnostics-for-method-calls
created_at: 2025-02-25T08:35:53Z
updated_at: 2025-02-25T14:17:51Z
url: https://github.com/astral-sh/ruff/pull/16362
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Better diagnostics for method calls

---

_@sharkdp_

## Summary

Add better error messages and additional spans for method calls. Can be reviewed commit-by-commit.

before:

![image](https://github.com/user-attachments/assets/11108693-0eed-4932-954c-acdb544cb45b)


after:

![image](https://github.com/user-attachments/assets/e57f7cae-a930-436d-90da-d4ae6ec98a83)


## Test Plan

New snapshot test

---

_Label `red-knot` added by @sharkdp on 2025-02-25 08:35_

---

_Review requested from @carljm by @sharkdp on 2025-02-25 08:35_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-25 08:35_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-25 08:35_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-02-25 08:46_

---

_@MichaReiser approved on 2025-02-25 08:47_

Nice

---

_Comment by @sharkdp on 2025-02-25 08:57_

I'm assuming the only thing worth discussing here is whether we really want to call it "bound method" or just "function" in the error message. There are also ways to extend what I started here (e.g. by providing better diagnostics for calls to classes, i.e. their `__init__` method), but I'd like to merge this as I have some other pending changes that "depend" on this to work (in the sense that we would otherwise see a regression in diagnostics messages after my semantic changes to some calls).

---

_Merged by @sharkdp on 2025-02-25 08:58_

---

_Closed by @sharkdp on 2025-02-25 08:58_

---

_Branch deleted on 2025-02-25 08:58_

---

_@BurntSushi reviewed on 2025-02-25 14:17_

Nice, LGTM!

---
