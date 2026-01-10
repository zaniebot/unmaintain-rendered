---
number: 1358
title: "Feature request: Ability to limit selected packages based on upload time"
type: issue
state: closed
author: notatallshaw
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T21:33:17Z
updated_at: 2024-02-22T03:05:19Z
url: https://github.com/astral-sh/uv/issues/1358
synced_at: 2026-01-10T01:23:05Z
---

# Feature request: Ability to limit selected packages based on upload time

---

_Issue opened by @notatallshaw on 2024-02-15 21:33_

Since PEP 700, Simple Index API 1.1 and above have supported the field `upload-time`. It would be useful for many use cases to support restricting what packages are installed by specifying an upper limit on this `upload-time`.

Particularly because:

1. Libraries are strongly discouraged from providing upper bounds.
2. Metadata cannot be changed once uploaded to PyPI.
3. Leading to the problem: If library A depends on library B and B releases a version that is incompatible with A, then a new version of A can get released that excludes the incompatible version of B, but this can mean that a package installer might just backtrack on A and install functionality incompatible versions of A and B even though their metadata doesn't describe it.

Therefore, installing older requirements can sometimes be significantly helped by adding an upper limit on upload-time. Or sometimes just the use case of "I ran these installs on this day and got a working environment, but today I don't".

Currently, there is a third-party package called `pypi-timemachine` to handle this, but it is quite slow and was created before PEP 700, and it uses the PyPI JSON API, which is a non-standard API: https://warehouse.pypa.io/api-reference/json.html. 

This feature is unlikely to ever be part of Pip as the maintainers think this feature is too orthogonal to Pip's goals, so if you are trying to feature match, then it's not something you should consider. But I would personally find it very helpful; it's hard to assess how big the broader impact would be.


---

_Comment by @charliermarsh on 2024-02-15 21:34_

Funny -- we actually do support this but it's hidden in the API since it originally existed for our own testing.

---

_Comment by @charliermarsh on 2024-02-15 21:35_

You can pass (e.g.) `--exclude-newer 2023-11-18T12:00:00Z` to `uv pip compile` or `uv pip install`.

---

_Comment by @zanieb on 2024-02-15 21:36_

It's feasible we'll make this public in the future. The error messages from it are not user friendly since we pretend any newer versions do not exist to make our tests reproducible over time.

---

_Label `enhancement` added by @zanieb on 2024-02-15 21:36_

---

_Referenced in [pypa/pip#12275](../../pypa/pip/issues/12275.md) on 2024-02-16 22:36_

---

_Closed by @charliermarsh on 2024-02-22 03:05_

---

_Comment by @charliermarsh on 2024-02-22 03:05_

(Closing since we do support this.)

---

_Referenced in [astral-sh/uv#2088](../../astral-sh/uv/issues/2088.md) on 2024-02-29 16:50_

---
