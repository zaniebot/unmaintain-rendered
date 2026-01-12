```yaml
number: 14758
title: "Disallow multiple `pylock.toml` files in `uv pip install` and `uv pip sync`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
base: main
head: charlie/one
created_at: 2025-07-20T17:59:37Z
updated_at: 2025-07-20T18:01:04Z
url: https://github.com/astral-sh/uv/pull/14758
synced_at: 2026-01-12T16:11:23Z
```

# Disallow multiple `pylock.toml` files in `uv pip install` and `uv pip sync`

---

_@charliermarsh_

## Summary

I think this is just an oversight. One of the files just gets ignored.


---

_Label `error messages` added by @charliermarsh on 2025-07-20 17:59_

---

_Marked ready for review by @charliermarsh on 2025-07-20 17:59_

---

_Comment by @charliermarsh on 2025-07-20 18:00_

Oh nevermind, we do have an error for this.

---

_Comment by @charliermarsh on 2025-07-20 18:01_

And tests, hah. Whoops.

---

_Closed by @charliermarsh on 2025-07-20 18:01_

---
