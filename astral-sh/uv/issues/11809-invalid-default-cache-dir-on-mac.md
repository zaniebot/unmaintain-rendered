```yaml
number: 11809
title: invalid default cache-dir on mac
type: issue
state: closed
author: luqasz
labels:
  - documentation
  - good first issue
  - question
assignees: []
created_at: 2025-02-26T19:18:06Z
updated_at: 2025-02-28T02:14:10Z
url: https://github.com/astral-sh/uv/issues/11809
synced_at: 2026-01-12T16:00:46Z
```

# invalid default cache-dir on mac

---

_@luqasz_

### Summary

```shell
# uv --no-config cache dir
/Users/lkostka/.cache/uv
lkostka@emjeden~/workspace/oss/librouteros (uv|+8d2)
```
As pointed in [documentation](https://docs.astral.sh/uv/reference/settings/#cache-dir), it should point to `$HOME/Library/Caches/uv` 

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.3 (Homebrew 2025-02-24)

### Python version

3.13

---

_Label `bug` added by @luqasz on 2025-02-26 19:18_

---

_Comment by @zanieb on 2025-02-26 19:34_

Hi! This is an intentional decision. We originally used `Library/Caches`, but that was more confusing to people than using a standard path for all Unix platforms.

---

_Label `bug` removed by @zanieb on 2025-02-26 19:34_

---

_Label `question` added by @zanieb on 2025-02-26 19:34_

---

_Comment by @zanieb on 2025-02-26 19:34_

ref https://github.com/astral-sh/uv/pull/5806

Looks like we missed a documentation update.

---

_Label `documentation` added by @zanieb on 2025-02-26 19:34_

---

_Label `good first issue` added by @zanieb on 2025-02-26 19:34_

---

_Comment by @luqasz on 2025-02-26 22:06_

hmm ok then. I'll add explicit `cache-dir` global option.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-28 02:04_

---

_Closed by @charliermarsh on 2025-02-28 02:14_

---
