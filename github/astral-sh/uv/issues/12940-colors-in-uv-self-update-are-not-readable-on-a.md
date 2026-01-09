---
number: 12940
title: "colors in `uv self update` are not readable on a light theme"
type: issue
state: closed
author: kdheepak
labels:
  - bug
assignees: []
created_at: 2025-04-17T12:27:03Z
updated_at: 2025-04-17T16:58:38Z
url: https://github.com/astral-sh/uv/issues/12940
synced_at: 2026-01-07T13:12:18-06:00
---

# colors in `uv self update` are not readable on a light theme

---

_Issue opened by @kdheepak on 2025-04-17 12:27_

### Summary

<img width="686" alt="Image" src="https://github.com/user-attachments/assets/06d09c6c-f229-4f55-92db-d8c97cb87351" />

<img width="386" alt="Image" src="https://github.com/user-attachments/assets/f80230bb-d82f-460b-bd7a-f6f37668ccde" />

Here's the following with a manual highlight using a selection with the mouse:

<img width="700" alt="Image" src="https://github.com/user-attachments/assets/62404e78-93a1-41f1-b0b9-07f8f2e2bed2" />

The offending lines appears to be these:

https://github.com/astral-sh/uv/blob/041c7a5e63d5559ce5571a30e177c072b1b357b2/crates/uv/src/commands/self_update.rs#L97-L103

https://github.com/astral-sh/uv/blob/041c7a5e63d5559ce5571a30e177c072b1b357b2/crates/uv/src/commands/self_update.rs#L130

It might be worth changing it to the following:

```diff
- .bold().white()
+ .bold().white().on_black()
```

or an equivalent.

### Platform

MacOS

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

Python 3.12.5

---

_Label `bug` added by @kdheepak on 2025-04-17 12:27_

---

_Comment by @charliermarsh on 2025-04-17 13:00_

Let's just change this to not use white. We don't really use white anywhere else, it feels out-of-sync with our style guide.

---

_Referenced in [astral-sh/uv#12943](../../astral-sh/uv/pulls/12943.md) on 2025-04-17 13:07_

---

_Closed by @charliermarsh on 2025-04-17 16:58_

---

_Closed by @charliermarsh on 2025-04-17 16:58_

---
