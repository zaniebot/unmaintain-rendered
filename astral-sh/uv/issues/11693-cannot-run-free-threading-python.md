```yaml
number: 11693
title: Cannot run free-threading Python
type: issue
state: closed
author: rinarakaki
labels:
  - bug
assignees: []
created_at: 2025-02-21T09:38:00Z
updated_at: 2025-02-21T09:42:07Z
url: https://github.com/astral-sh/uv/issues/11693
synced_at: 2026-01-12T16:00:44Z
```

# Cannot run free-threading Python

---

_@rinarakaki_

### Summary

By following the steps from https://x.com/charliermarsh/status/1845944897515237539, non-free-threading version of Python starts:

```sh
uv python install 3.13t
uv init --python 3.13t
uv run python
```

Output:

```
Python 3.13.2 (main, Feb 12 2025, 14:38:11) [GCC 6.3.0 20170516] on linux
```

### Platform

Linux 6.12.5-linuxkit aarch64 GNU/Linux

### Version

uv 0.6.2

### Python version

_No response_

---

_Label `bug` added by @rinarakaki on 2025-02-21 09:38_

---

_Comment by @rinarakaki on 2025-02-21 09:42_

Solved: `uv python pin 3.13t`

---

_Closed by @rinarakaki on 2025-02-21 09:42_

---
