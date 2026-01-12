```yaml
number: 475
title: "pip-sync doesn't checked if a remote url distribution has changed"
type: issue
state: closed
author: konstin
labels:
  - bug
  - security
assignees: []
created_at: 2023-11-21T13:23:54Z
updated_at: 2025-12-02T18:52:26Z
url: https://github.com/astral-sh/uv/issues/475
synced_at: 2026-01-12T15:58:23Z
```

# pip-sync doesn't checked if a remote url distribution has changed

---

_@konstin_

When using a remote distribution with pip-sync, e.g. `theano @ https://github.com/Theano/Theano/archive/master.zip`, we will accept any distribution that was installed from that url instead of checking that the installed distribution matches the remote distribution.

`direct_url.json` has a `hashes` key for remote distributions we can use to implement this correctly: When downloading a distribution, record its hash (sha256 probably) in the cache and check whether it matches the entry in `direct_url.json`. On each run (both `pip-sync` and `pip-compile`), check if the remote distribution changed and in that case, invalid the cached hash and fetch the metadata again (`pip-compile`) or reinstall (`pip-sync`)

---

_Comment by @zanieb on 2024-07-01 21:32_

@konstin is this still an issue?

---

_Label `bug` added by @zanieb on 2024-07-01 21:32_

---

_Label `security` added by @zanieb on 2024-07-01 21:32_

---

_Comment by @charliermarsh on 2024-07-01 22:47_

Yup!

---

_Label `help wanted` added by @zanieb on 2024-07-02 01:35_

---

_Label `help wanted` removed by @charliermarsh on 2024-11-23 03:22_

---

_Comment by @zanieb on 2025-11-08 19:09_

I presume this is a problem for all operations, not just the `uv pip sync` interface?

---

_Comment by @konstin on 2025-12-02 18:46_

From a quick check with a local `python -m http.server` webserver, both `uv pip install` and `uv sync` use a cached URL unless `--refresh` is given. `uv sync` correctly refreshes with `--no-cache`. Given that we now have `--refresh` and are consistent about it, this seems fine.

---

_Closed by @zanieb on 2025-12-02 18:52_

---
