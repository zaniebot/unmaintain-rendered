---
number: 13673
title: Local dev dependencies always installed with --all-packages
type: issue
state: closed
author: Knut-Boost
labels:
  - bug
assignees: []
created_at: 2025-05-27T08:38:54Z
updated_at: 2025-05-27T12:43:08Z
url: https://github.com/astral-sh/uv/issues/13673
synced_at: 2026-01-10T01:25:36Z
---

# Local dev dependencies always installed with --all-packages

---

_Issue opened by @Knut-Boost on 2025-05-27 08:38_

### Summary

Local packages included as dev dependencies are, under circumstances hard to describe precisely, reinstalled for each invocation of `uv sync --all-packages`. This is annoying as uv otherwise is delightfully fast.

Here is a small example: https://github.com/Knut-Boost/uv-example

### Platform

Ubuntu 22.04.5 LTS

### Version

0.7.8

### Python version

3.12.6

---

_Label `bug` added by @Knut-Boost on 2025-05-27 08:38_

---

_Comment by @charliermarsh on 2025-05-27 11:35_

My guess is you're triggering a cache invalidation, e.g., by modifying the `pyproject.toml`? If you see a reinstall _every_ time, that would be a problem. We'll need a clear reproduction to help you, though. See: https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/.

---

_Label `bug` removed by @charliermarsh on 2025-05-27 11:35_

---

_Label `needs-mre` added by @charliermarsh on 2025-05-27 11:35_

---

_Comment by @Knut-Boost on 2025-05-27 11:40_

I am seeing a reinstall _every_ time with the state in the example repository.

---

_Comment by @charliermarsh on 2025-05-27 11:43_

Thanks, I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-27 11:43_

---

_Comment by @Knut-Boost on 2025-05-27 12:18_

Thank you for looking into it. I added a Dockerfile to the example repo. Let me know if further clarification is needed.

---

_Referenced in [astral-sh/uv#13678](../../astral-sh/uv/pulls/13678.md) on 2025-05-27 12:31_

---

_Comment by @charliermarsh on 2025-05-27 12:32_

Thanks, it was a great reproduction -- I just missed it on first read, my fault. This turned out to be a super subtle issue that required (e.g.) that the workspace root name was alphabetically lower than the member, etc. Fixed it here: https://github.com/astral-sh/uv/pull/13678.

---

_Label `needs-mre` removed by @charliermarsh on 2025-05-27 12:32_

---

_Label `bug` added by @charliermarsh on 2025-05-27 12:32_

---

_Comment by @Knut-Boost on 2025-05-27 12:33_

Amazing, thanks for the swift resolution!

---

_Closed by @charliermarsh on 2025-05-27 12:43_

---
