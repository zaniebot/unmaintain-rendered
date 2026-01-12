```yaml
number: 4776
title: "`uv run` can fail to invoke `python`"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-03T15:11:03Z
updated_at: 2024-07-06T19:22:20Z
url: https://github.com/astral-sh/uv/issues/4776
synced_at: 2026-01-12T15:58:52Z
```

# `uv run` can fail to invoke `python`

---

_@zanieb_

```
❯ uv run --isolated -v crates/uv-python/fetch-download-metadata.py
DEBUG uv 0.2.21
warning: `uv run` is experimental and may change without warning.
DEBUG Running `python crates/uv-python/fetch-download-metadata.py`
error: Failed to spawn: `python`
  Caused by: No such file or directory (os error 2)
  ```

---

_Label `bug` added by @zanieb on 2024-07-03 15:11_

---

_Label `preview` added by @zanieb on 2024-07-03 15:11_

---

_Comment by @charliermarsh on 2024-07-03 15:15_

What's going on here? Hah

---

_Comment by @charliermarsh on 2024-07-05 22:10_

Should we use the absolute path to the Python interpreter that we found?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-05 22:10_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-07-05 22:10_

---

_Comment by @charliermarsh on 2024-07-05 22:10_

Or are we not adding it to PATH, maybe?

---

_Comment by @charliermarsh on 2024-07-05 22:11_

Hmm, no, we do add the scripts directory to `PATH` which should include `python`...

---

_Comment by @zanieb on 2024-07-05 23:15_

My suspicion here, without investigating further, is that we didn't create an environment (since there are no requirements) and tried to invoke `python` outside of one?

---

_Comment by @charliermarsh on 2024-07-06 19:06_

Yeah this makes sense. It's kinda the same as https://github.com/astral-sh/uv/issues/4846.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-06 19:06_

---

_Closed by @charliermarsh on 2024-07-06 19:22_

---

_Closed by @charliermarsh on 2024-07-06 19:22_

---
