```yaml
number: 5747
title: "`Ctrl-C` interruptes `uv python install` and leaves temporary files"
type: issue
state: closed
author: j178
labels: []
assignees: []
created_at: 2024-08-03T13:00:48Z
updated_at: 2024-08-05T02:32:25Z
url: https://github.com/astral-sh/uv/issues/5747
synced_at: 2026-01-12T15:58:58Z
```

# `Ctrl-C` interruptes `uv python install` and leaves temporary files

---

_@j178_

`uv python install` creates a temporary directory at `$HOME/Library/Application Support/uv/python`. If the process is interrupted with `Ctrl-C`, the temporary files are not deleted, resulting in numerous hidden directories left behind. It might be beneficial to change the temporary download location to the system-level `/tmp`, which is periodically cleaned by the system.

---

_Renamed from "<kbd>ctrl-c</kbd> interruptes `uv python install` and leaves temporary files" to "`Ctrl-C` interruptes `uv python install` and leaves temporary files" by @j178 on 2024-08-03 13:01_

---

_Comment by @charliermarsh on 2024-08-03 13:02_

Using `/tmp` has other problems, namely that the temporary directories won't be colocated with the final locations. This can cause problems, for example, you can't hardlink between them if they happen to be on separate filesystems. So we tend to use temporary directories colocated with wherever we're writing.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-05 02:19_

---

_Closed by @charliermarsh on 2024-08-05 02:32_

---
