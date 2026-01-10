---
number: 17337
title: "feat: implement `--upgrade-strategy` for uv pip install"
type: issue
state: open
author: ismxilxrif
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2026-01-06T15:31:01Z
updated_at: 2026-01-06T18:08:38Z
url: https://github.com/astral-sh/uv/issues/17337
synced_at: 2026-01-10T01:26:16Z
---

# feat: implement `--upgrade-strategy` for uv pip install

---

_Issue opened by @ismxilxrif on 2026-01-06 15:31_

### Summary

```
  --upgrade-strategy <upgrade_strategy>
                              Determines how dependency upgrading should be handled [default: only-if-needed]. "eager" - dependencies are upgraded regardless of whether the currently
                              installed version satisfies the requirements of the upgraded package(s). "only-if-needed" -  are upgraded only when they do not satisfy the requirements of
                              the upgraded package(s).
```

currently `uv pip install --upgrade somepackage` behaves like eager, but pip defaults to only-if-needed, so that it won't break previous package installations if they lock it to say, 1.2.x but the newer command would force it to 1.3.x.

### Example

_No response_

---

_Label `enhancement` added by @ismxilxrif on 2026-01-06 15:31_

---

_Renamed from "feat: implement --upgrade-strategy for uv pip install" to "feat: implement `--upgrade-strategy` for uv pip install" by @ismxilxrif on 2026-01-06 15:31_

---

_Label `compatibility` added by @konstin on 2026-01-06 18:08_

---
