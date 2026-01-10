```yaml
number: 1791
title: Add inlay hints for infered types on marker special types
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-06T01:12:20Z
updated_at: 2025-12-06T01:28:05Z
url: https://github.com/astral-sh/ty/issues/1791
synced_at: 2026-01-10T01:56:41Z
```

# Add inlay hints for infered types on marker special types

---

_Issue opened by @MeGaGiGaGon on 2025-12-06 01:12_

There are some special types like `Final` and `ClassVar` that are only markers and don't actually affect the type, which means even with a partial type annotation `ty` still does inference (unlike normal generic types like `list` where `x: list` means `list[Any`). It would be nice if there was an inlay hint showing the inferred type on these special statements like how `BasedPyright` does it in VSC:

<img width="217" height="32" alt="Image" src="https://github.com/user-attachments/assets/6e4499fc-30f4-4e16-8f3e-9484d26f6ce7" />

---

_Comment by @Gankra on 2025-12-06 01:27_

In this specific case we refuse to show an inlay hint even if you don't write Final because we don't find it helpful to tell you =1 assigned Literal[1], but I guess we could for the non-trivial cases?

---

_Label `server` added by @Gankra on 2025-12-06 01:27_

---

_Label `wish` added by @Gankra on 2025-12-06 01:28_

---
