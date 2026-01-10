```yaml
number: 4432
title: Unable to release due to PyPI project size limits
type: issue
state: closed
author: zanieb
labels:
  - releases
assignees: []
created_at: 2024-06-20T21:40:39Z
updated_at: 2024-06-24T13:24:48Z
url: https://github.com/astral-sh/uv/issues/4432
synced_at: 2026-01-10T05:31:37Z
```

# Unable to release due to PyPI project size limits

---

_Issue opened by @zanieb on 2024-06-20 21:40_

As previously encountered in Ruff https://github.com/astral-sh/ruff/issues/2136, we've reached the default project size limit on PyPI.

Unfortunately this was encountered in the middle of the 0.2.14 release, which is now _partially_ available. We're determining the next steps for that release, as only the `macOS 10.12+ x86-64` and `armv6l` wheels are uploaded (and no source distribution).

We'll be requesting a size increase and reporting progress here. Thanks!

---

_Label `release` added by @zanieb on 2024-06-20 21:40_

---

_Comment by @zanieb on 2024-06-20 21:53_

The size increase can be followed at https://github.com/pypi/support/issues/4260

---

_Comment by @zanieb on 2024-06-20 21:57_

I've made the decision to yank 0.2.14 from PyPI. I do not believe any other artifacts are available and verified that our self-update and standalone installer both pull 0.2.13 still.

---

_Comment by @zanieb on 2024-06-21 17:52_

I've noticed the [Docker image](https://github.com/astral-sh/uv/pkgs/container/uv) is available at 0.2.14 but I do not intend to remove it.

---

_Comment by @charliermarsh on 2024-06-24 13:24_

Limit has been raised: https://github.com/pypi/support/issues/4260. Thanks @zanieb for initiating and to PyPI for the increase!

---

_Closed by @charliermarsh on 2024-06-24 13:24_

---
