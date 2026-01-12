```yaml
number: 833
title: "Improve cache performance by removing `cacache` dependency"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cache-speed
created_at: 2022-11-20T17:36:52Z
updated_at: 2022-11-20T18:36:34Z
url: https://github.com/astral-sh/ruff/pull/833
synced_at: 2026-01-12T05:48:45Z
```

# Improve cache performance by removing `cacache` dependency

---

_Pull request opened by @charliermarsh on 2022-11-20 17:36_

In the CPython benchmark (with `--no-cache`), this brings performance down to ~70-72ms, from ~92ms -- so something like a 20% speedup.

Resolves #230.


---

_Comment by @charliermarsh on 2022-11-20 17:38_

I also looked at swapping `bincode` out for a faster serialization library, but it turns out that serialization is an extremely small part of the execution time here. (On `main`, locating all the files to lint on-disk is about ~18ms, reading from the cache is about ~58ms...)

---

_Comment by @charliermarsh on 2022-11-20 17:40_

We're not using the content-addressable nature of `cacache`, so it's kind of wasteful. It also makes Wasm builds more complex right now.

---

_Merged by @charliermarsh on 2022-11-20 18:36_

---

_Closed by @charliermarsh on 2022-11-20 18:36_

---

_Branch deleted on 2022-11-20 18:36_

---
