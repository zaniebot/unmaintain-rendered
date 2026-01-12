```yaml
number: 1919
title: Minor updates to the docs
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
assignees: []
merged: true
base: main
head: david/minor-docs-updates
created_at: 2025-12-16T10:07:36Z
updated_at: 2025-12-16T10:32:16Z
url: https://github.com/astral-sh/ty/pull/1919
synced_at: 2026-01-12T15:54:28Z
```

# Minor updates to the docs

---

_@sharkdp_

## Summary

I did a full review of all docs using Claude, and these seemed worth fixing.



---

_Label `documentation` added by @sharkdp on 2025-12-16 10:07_

---

_@sharkdp reviewed on 2025-12-16 10:08_

---

_Review comment by @sharkdp on `docs/suppression.md`:95 on 2025-12-16 10:08_

I was confused when reading this example because I focused on the (apparently) wrong code in `add_three`, while I should be focusing on the wrong call-site in `main`. I guess this is just a Rust-vs-Python issue and this is what was actually meant.

---

_@sharkdp reviewed on 2025-12-16 10:09_

---

_Review comment by @sharkdp on `docs/suppression.md`:99 on 2025-12-16 10:09_

Changing arguments to `1, 2` to make it even more apparent that one is missing

---

_Review comment by @AlexWaygood on `docs/suppression.md`:95 on 2025-12-16 10:10_

My intuition would be that an `add_three` function would take a single integer and add `3` to it. So we could also consider renaming it

```suggestion
def sum_three_ints(a: int, b: int, c: int) -> int:
    return a + b + c
```

(need to also update the use of the function a few lines below, if you accept this)

---

_Review comment by @AlexWaygood on `docs/type-checking.md`:27 on 2025-12-16 10:12_

Maybe worth mentioning that it recurses into subdirectories of subdirectories?

```suggestion
ty will run on all Python files in the working directory and its (recursive) subdirectories. If used from a
```

---

_@AlexWaygood approved on 2025-12-16 10:13_

---

_Merged by @sharkdp on 2025-12-16 10:32_

---

_Closed by @sharkdp on 2025-12-16 10:32_

---

_Branch deleted on 2025-12-16 10:32_

---
