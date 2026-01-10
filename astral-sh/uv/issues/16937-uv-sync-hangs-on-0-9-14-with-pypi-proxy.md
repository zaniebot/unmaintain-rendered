---
number: 16937
title: "`uv sync` hangs on 0.9.14 with PyPI proxy"
type: issue
state: closed
author: georgeflug
labels:
  - bug
assignees: []
created_at: 2025-12-02T18:39:48Z
updated_at: 2025-12-06T01:51:45Z
url: https://github.com/astral-sh/uv/issues/16937
synced_at: 2026-01-10T01:26:11Z
---

# `uv sync` hangs on 0.9.14 with PyPI proxy

---

_Issue opened by @georgeflug on 2025-12-02 18:39_

### Summary

This issue started happening on 0.9.14. It works OK on 0.9.13 or earlier.

When running `uv sync`, the command hangs indefinitely. This occurs when connecting to a PyPI proxy on Sonatype Nexus - it does not occur when using the PyPI index directly.

Repro steps:
1. Install uv 0.9.14
2. Clear caches and sync against PyPI (works OK): `rm -rf .venv ~/.cache/uv && uv sync -vv`
3. Clear caches and sync against Nexus (hangs): `rm -rf .venv ~/.cache/uv && uv sync --index="https://nexus.mycompany.com/repository/PyPI/simple/" -vv`

From the logs (see attached), it looks like it's waiting for a response for the `hatchling` metadata because I don't see the following entry when it hangs: `TRACE Received package metadata for: hatchling`

[uv_sync_0.9.13.txt](https://github.com/user-attachments/files/23888358/uv_sync_0.9.13.txt)
[uv_sync_0.9.14.txt](https://github.com/user-attachments/files/23888357/uv_sync_0.9.14.txt)

### Platform

macOs and Ubuntu

### Version

0.9.14

### Python version

3.11.14

---

_Label `bug` added by @georgeflug on 2025-12-02 18:39_

---

_Comment by @zanieb on 2025-12-02 18:47_

Thanks for the report. Would you be willing to test a branch with a fix?

The changes between 13 and 14 are https://github.com/astral-sh/uv/compare/0.9.13...0.9.14

Only #16887 looks suspicious but even that's a stretch.

---

_Referenced in [astral-sh/uv#16938](../../astral-sh/uv/pulls/16938.md) on 2025-12-02 18:52_

---

_Comment by @georgeflug on 2025-12-02 23:47_

The branch fixes it!

Interestingly, the branch and even the older 0.9.13 still have the problem if I run the build as `cargo build -p uv`. But when I run the build as `cargo build --release`, then 0.9.13, the branch, and everything leading up to f2c33b4 are good, and f2c33b4 through 0.9.14 are bad.

---

_Comment by @zanieb on 2025-12-02 23:50_

Interesting. Thanks for giving it a try!

A revert will go out in the next release and we can investigate.

Would you be able to share the html response from the index? I'm not quite sure how we're going to reproduce the problem.

---

_Comment by @georgeflug on 2025-12-02 23:53_

Thanks for reverting! Yes I can. What URL do you want to see?

---

_Comment by @zanieb on 2025-12-03 00:06_

I believe the https://pypi.org/simple/hatchling/ equivalent is what we need. Does this happen for arbitrary packages?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-03 05:52_

---

_Comment by @charliermarsh on 2025-12-05 03:45_

When possible, can you please follow-up with a response that we can use to reproduce this? Otherwise, we risk breaking you again in a subsequent PR, since we'll have no way of knowing if we've actually fixed your issue.

---

_Referenced in [astral-sh/astral-tl#16](../../astral-sh/astral-tl/pulls/16.md) on 2025-12-05 04:01_

---

_Comment by @georgeflug on 2025-12-06 00:56_

Sorry for the delay. See the attached request/response pulled from mitmproxy while uv sync ran. I was able to reproduce this with other packages besides hatchling, in this case, pyt.

[pyt.log](https://github.com/user-attachments/files/23973007/pyt.log)

The `<script>` tag at the bottom seems like a likely culprit since that's not present on pypi.org.

---

_Comment by @charliermarsh on 2025-12-06 00:59_

Amazing, thank you.

---

_Referenced in [astral-sh/uv#17010](../../astral-sh/uv/pulls/17010.md) on 2025-12-06 01:51_

---

_Closed by @charliermarsh on 2025-12-06 01:51_

---
