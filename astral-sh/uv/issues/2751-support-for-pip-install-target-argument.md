---
number: 2751
title: Support for pip install --target argument
type: issue
state: closed
author: mpderbec
labels:
  - compatibility
assignees: []
created_at: 2024-04-01T03:44:35Z
updated_at: 2024-04-01T03:46:41Z
url: https://github.com/astral-sh/uv/issues/2751
synced_at: 2026-01-10T01:23:21Z
---

# Support for pip install --target argument

---

_Issue opened by @mpderbec on 2024-04-01 03:44_

We have some specific tooling that uses the `-t` flag in `pip install` to direct where we want pip to put the packages. (We aren't using virtual environments.)

Here's the help from pip for that argument:

```
-t, --target <dir>          Install packages into <dir>. By default this will not replace existing files/folders in <dir>. Use --upgrade to replace existing packages in <dir> with new versions.
```

Not sure how popular the argument is in pip land, but at least for us I've not found a way to soldier on without it.

---

_Comment by @charliermarsh on 2024-04-01 03:46_

Do you mind chiming in here? https://github.com/astral-sh/uv/issues/1517

Merging into that issue.

---

_Closed by @charliermarsh on 2024-04-01 03:46_

---

_Label `compatibility` added by @charliermarsh on 2024-04-01 03:46_

---
