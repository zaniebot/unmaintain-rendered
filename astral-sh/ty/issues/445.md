```yaml
number: 445
title: Before posting in the issue tracker
type: issue
state: closed
author: sharkdp
labels: []
assignees: []
created_at: 2025-05-19T11:56:41Z
updated_at: 2025-12-08T13:46:24Z
url: https://github.com/astral-sh/ty/issues/445
synced_at: 2026-01-10T01:56:40Z
```

# Before posting in the issue tracker

---

_Issue opened by @sharkdp on 2025-05-19 11:56_

Thank you for engaging with ty!

Before opening an issue on this repository, note that ty is currently in preview and not ready for production use.

That said, we are very much interested in getting feedback. Before opening a new ticket, please search the issue tracker for existing issues. In particular, please take note of the following known ty issues that users are likely to run into on larger projects — if your issue is captured here, we're aware of it and there's no need to open a new issue. Please add a 👍 reaction on the top comment rather than commenting with "me too", etc.

- **[Fatal errors for self-self-referential (recursive) generic classes, type aliases, or protocols](https://github.com/astral-sh/ty/issues/256)**
  ty currently crashes for some self-referential constructs using generic classes, type aliases or protocols.

  Common symptoms:
    * ty crashes with *"A fatal error occurred while checking some files"*. The panic message (search for "Panicked at") includes the message **"too many cycle iterations"**
    * Same as above, but the message includes **"dependency graph cycle"** instead.


---

_Locked by @astral-sh on 2025-05-19 12:03_

---

_Added to milestone `Beta` by @carljm on 2025-11-14 14:44_

---

_Closed by @sharkdp on 2025-12-08 13:46_

---
