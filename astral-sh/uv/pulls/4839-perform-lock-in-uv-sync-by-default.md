```yaml
number: 4839
title: "Perform lock in `uv sync` by default"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/lock-sync
created_at: 2024-07-05T21:43:57Z
updated_at: 2024-07-09T22:18:32Z
url: https://github.com/astral-sh/uv/pull/4839
synced_at: 2026-01-12T16:06:29Z
```

# Perform lock in `uv sync` by default

---

_@charliermarsh_

## Summary

- `uv sync` will now lock by default.
- `uv sync --locked` will lock, and error if the generated lock does not match `uv.lock` on-disk.
- `uv sync --frozen` will skip locking and just use `uv.lock`.

Closes https://github.com/astral-sh/uv/issues/4812.
Closes https://github.com/astral-sh/uv/issues/4803.


---

_Label `preview` added by @charliermarsh on 2024-07-05 21:44_

---

_Comment by @charliermarsh on 2024-07-05 21:49_

Needs tests.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-05 21:56_

---

_Review requested from @konstin by @charliermarsh on 2024-07-05 21:56_

---

_Comment by @konstin on 2024-07-06 21:13_

Shouldn't locked check that the lock fulfills the requirements, as opposed to that the lock is the latest possible lock wrt to pypi?

What are the use cases of `--frozen` where you wouldn't use `--locked`?


---

_Comment by @charliermarsh on 2024-07-06 21:49_

I thought locked was about ensuring that the lockfile wouldn’t be updated by a “lock” operation. We don’t upgrade without —upgrade anyway.

---

_Comment by @charliermarsh on 2024-07-06 21:55_

I think having an operation that only installs, and doesn’t resolve at all, seems really useful! Even just for my own sanity and testing.

---

_Comment by @konstin on 2024-07-07 09:34_

I see, i got confused because we don't pass `existing_lock` to the locking process, but that has another `uv.lock` read to extract preferences.

---

_@konstin approved on 2024-07-07 09:35_

---

_Comment by @charliermarsh on 2024-07-09 16:09_

@konstin - I'll change it such that we pass the lockfile to that call, to make it clearer / less implicit (and also more efficient).

---

_Merged by @charliermarsh on 2024-07-09 22:18_

---

_Closed by @charliermarsh on 2024-07-09 22:18_

---

_Branch deleted on 2024-07-09 22:18_

---
