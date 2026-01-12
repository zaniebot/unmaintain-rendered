```yaml
number: 5969
title: Support relative path wheels
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/relative-wheel-paths
created_at: 2024-08-09T17:37:58Z
updated_at: 2024-08-09T21:57:17Z
url: https://github.com/astral-sh/uv/pull/5969
synced_at: 2026-01-12T16:07:08Z
```

# Support relative path wheels

---

_@konstin_

Surprisingly, this is a lockfile schema change: We can't store relative paths in urls, so we have to store a `filename` entry instead of the whole url.

Fixes #4355


---

_Label `enhancement` added by @konstin on 2024-08-09 17:37_

---

_Label `preview` added by @konstin on 2024-08-09 17:37_

---

_Comment by @charliermarsh on 2024-08-09 17:39_

Why do we drop the hash?

---

_Comment by @konstin on 2024-08-09 17:41_

If the hash is important, i can add a new `wheels` entry variant. Currently, it's dropped because we can't represent the relative path as the `url` that `wheels` entries use.

It's the thing i asked about in chat:

```toml
[[package]]
name = "ok"
version = "1.0.0"
source = { path = "wheels/ok-1.0.0-py3-none-any.whl" }
wheels = [
    { url = "file:///tmp/.tmpZaMe4t/wheels/ok-1.0.0-py3-none-any.whl", hash = "sha256:79f0b33e6ce1e09eaa1784c8eee275dfe84d215d9c65c652f07c18e85fdaac5f" },
]
```

---

_Comment by @charliermarsh on 2024-08-09 18:05_

I think it's worth adding two variants to wheels.

---

_Comment by @konstin on 2024-08-09 19:30_

We don't drop the hash anymore.

---

_@charliermarsh approved on 2024-08-09 19:32_

---

_Comment by @charliermarsh on 2024-08-09 19:32_

Thanks, this looks good. It would be nice to remove the duplicated filename but... not critical.

---

_Comment by @charliermarsh on 2024-08-09 20:15_

Debugging test failure, then will merge.

---

_Comment by @charliermarsh on 2024-08-09 20:26_

Ok this is an actual Windows bug. We're caching the file under `file:///C:/Users/runneradmin/AppData/Local/Temp/.tmppQrNqo/wheels/ok-1.0.0-py3-none-any.whl`, but we then look for it later under `file:///C:/Users/RUNNER~1/AppData/Local/Temp/.tmppQrNqo/wheels/ok-1.0.0-py3-none-any.whl`.

---

_Comment by @charliermarsh on 2024-08-09 21:48_

I'm going to skip this test on Windows while I fix it.

---

_Comment by @charliermarsh on 2024-08-09 21:50_

(Just hid the counts and added a TODO.)

---

_Merged by @charliermarsh on 2024-08-09 21:57_

---

_Closed by @charliermarsh on 2024-08-09 21:57_

---

_Branch deleted on 2024-08-09 21:57_

---
