```yaml
number: 734
title: Dir-file conflict
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: dir-file-conflicting
created_at: 2023-12-27T20:11:16Z
updated_at: 2024-01-22T09:48:54Z
url: https://github.com/astral-sh/uv/pull/734
synced_at: 2026-01-10T15:39:02Z
```

# Dir-file conflict

---

_Pull request opened by @konstin on 2023-12-27 20:11_

```
virtualenv --clear -p 3.10 .venv -q
cargo run --bin puffin -q -- pip-sync scripts/dir_file_conflict/requirements.txt
```

fails with

```
error: Failed to install: has_file-0.1.0-py3-none-any.whl (has-file==0.1.0 (from file:///home/konsti/projects/puffin/scripts/dir_file_conflict/has_file/dist/has_file-0.1.0-py3-none-any.whl))
  Caused by: failed to remove file `/home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/requirements/conflicting`
  Caused by: Is a directory (os error 21)
```

---

_Converted to draft by @konstin on 2023-12-27 20:11_

---

_Comment by @konstin on 2024-01-22 09:48_

The error is now slightly better:
```
$ cargo run --bin puffin -q -- pip sync scripts/dir_file_conflict/requirements.txt
Resolved 2 packages in 0ms
Downloaded 2 packages in 2ms
error: Failed to install: has_file-0.1.0-py3-none-any.whl (has-file==0.1.0 (from file:///home/konsti/projects/puffin/scripts/dir_file_conflict/has_file/dist/has_file-0.1.0-py3-none-any.whl))
  Caused by: failed to rename file from /home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/.tmpNqOmQp/conflicting to /home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/requirements/conflicting
  Caused by: Is a directory (os error 21)
```

Imho that's a fringe enough case (i don't think i've ever seen it except in the ecosystem install check) to not special case the error message.

---

_Closed by @konstin on 2024-01-22 09:48_

---
