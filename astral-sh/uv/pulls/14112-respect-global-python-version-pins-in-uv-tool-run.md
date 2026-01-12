```yaml
number: 14112
title: "Respect global Python version pins in `uv tool run` and `uv tool install`"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/tool-pyver
created_at: 2025-06-17T17:27:18Z
updated_at: 2025-10-08T15:04:52Z
url: https://github.com/astral-sh/uv/pull/14112
synced_at: 2026-01-12T16:11:02Z
```

# Respect global Python version pins in `uv tool run` and `uv tool install`

---

_@zanieb_

Closes #12921 

For `uv tool run`, we'll just use the global Python version for all invocations without an explicit alternative request (i.e., via the `--python` flag).

For `uv tool install`, it's a bit more complicated:

- If the tool is not installed, we'll use the global Python version
- If the tool is already installed, we won't change the Python version unless `--reinstall` or `--python` is used
- If the tool was installed with `--python`, we won't use the global Python version, unless the tool is uninstalled first

The behavior can be demonstrated as follows

```
$ uv python pin --global 3.12
$ uv tool install flask  # uses 3.12
$ uv tool install flask  # no-op
$ uv python pin --global 3.13
$ uv tool install flask  # no-op
$ uv tool install flask --reinstall  # uses 3.13
$ uv tool install flask -p 3.12 # uses 3.12
$ uv tool install flask  # no-op
$ uv tool install flask --reinstall # uses 3.12
```

This is a little more complicated than always reinstalling when the global Python version pin changes, but I think it's probably more intuitive when actually using the tool. We briefly touched on this when adding global version pins at https://github.com/astral-sh/uv/pull/12115#discussion_r1992222278

Minor note: I need to do a self-review of this implementation, as it's a little awkward to encode this behavior in the existing logic.


---

_Label `bug` added by @zanieb on 2025-06-17 17:27_

---

_@zanieb reviewed on 2025-06-18 23:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:401 on 2025-06-18 23:52_

I think this could be refactored to avoid the repeated `else` branches, but the logic should be correct. The comment needs more detail.

---

_Comment by @zanieb on 2025-06-20 20:33_

@jtfmumm want to give this a look and see if we're aligned on the behavior?

---

_Comment by @jtfmumm on 2025-06-26 12:00_

> @jtfmumm want to give this a look and see if we're aligned on the behavior?

The behavior makes sense to me, and is consistent with how I was thinking about it before.



---
