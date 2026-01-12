```yaml
number: 7708
title: "Check in test script for uv run <url>"
type: pull_request
state: merged
author: manzt
labels: []
assignees: []
merged: true
base: main
head: manzt/remote-script
created_at: 2024-09-26T13:53:47Z
updated_at: 2024-09-26T15:05:39Z
url: https://github.com/astral-sh/uv/pull/7708
synced_at: 2026-01-12T16:07:57Z
```

# Check in test script for uv run <url>

---

_@manzt_

Merge before #6375 (see https://github.com/astral-sh/uv/pull/6375#discussion_r1776975298)

This is testing a couple of things at once, and I'm happy to strip back to something more simple (i.e., "just" printing).
However, I figured it would be useful to ensure that the PEP 723 metadata correctly works as well as that 
additional arguments are correctly forwarded to the Python program.

cc: @burntsushi


---

_Review comment by @BurntSushi on `scripts/uv-run-remote-script-test.py`:5 on 2024-09-26 13:59_

Can you do `rich==13.8.1`? Just so the dependency is pinned.

---

_@BurntSushi requested changes on 2024-09-26 14:00_

This LGTM with a version pin and when CI passes. It looks like a Python lint violation?

---

_Comment by @BurntSushi on 2024-09-26 14:00_

> However, I figured it would be useful to ensure that the PEP 723 metadata correctly works as well as that
additional arguments are correctly forwarded to the Python program.

Yeah that SGTM!

---

_@zanieb approved on 2024-09-26 14:51_

---

_@BurntSushi approved on 2024-09-26 15:00_

---

_Merged by @BurntSushi on 2024-09-26 15:00_

---

_Closed by @BurntSushi on 2024-09-26 15:00_

---

_Branch deleted on 2024-09-26 15:05_

---
