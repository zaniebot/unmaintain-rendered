```yaml
number: 15602
title: UI for gui apps completely borked
type: issue
state: open
author: RuggedPineapple
labels:
  - bug
assignees: []
created_at: 2025-08-31T04:40:05Z
updated_at: 2025-09-01T07:36:27Z
url: https://github.com/astral-sh/uv/issues/15602
synced_at: 2026-01-12T16:02:13Z
```

# UI for gui apps completely borked

---

_@RuggedPineapple_

### Summary

When using the built-in gui frameworks uv installed python versions produce something that, depending on exactly whats being drawn, is somewhere between 'ugly as sin' and 'completely unusable'

Needed to use uv for an app that maxes out at python 3.12 and ubuntu 25.04 ships with 3.13 as well as being unsupported by deadsnakes so used uv to install 3.12. After seeing the issue tried 3.10. Both versions demonstrated the same issue. I modified the version check in the app to get it to load in the system python which wont actually run but does at least let me get into the set up UI to show what it's intended to look like.

UI with system python:
<img width="1169" height="822" alt="Image" src="https://github.com/user-attachments/assets/05a24614-04eb-47b7-a7ff-205edc79215e" />

UI with uv installed python:
<img width="1169" height="822" alt="Image" src="https://github.com/user-attachments/assets/9d50af3b-7d16-4d31-b9f9-e95a9ae5ed1a" />


As you can see, the buttons have corners cut out of them, the dropdown arrows are Ys and I can't tell what switches are active.

### Platform

Ubuntu 25.04

### Version

uv 0.8.14

### Python version

tested in cpython-3.10.18-linux-x86_64-gnu  and cpython-3.12.11-linux-x86_64-gnu

---

_Label `bug` added by @RuggedPineapple on 2025-08-31 04:40_

---

_Comment by @konstin on 2025-09-01 06:38_

Probably a duplicate of #14042

---

_Comment by @RuggedPineapple on 2025-09-01 07:28_

I did see that issue, which is why I didn't mention the fonts being borked as well so as not to clog the issue tracker. But since it didn't mention UI issues figured I should open this issue. Would it be better for me to close the issue?

---

_Comment by @konstin on 2025-09-01 07:36_

CC @geofft for identifying whether that's the exact same problem.

---
