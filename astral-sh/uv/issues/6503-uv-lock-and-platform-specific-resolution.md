---
number: 6503
title: "`uv lock` and platform-specific resolution"
type: issue
state: closed
author: SamyAB
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-23T09:45:05Z
updated_at: 2024-12-26T16:53:07Z
url: https://github.com/astral-sh/uv/issues/6503
synced_at: 2026-01-10T01:24:02Z
---

# `uv lock` and platform-specific resolution

---

_Issue opened by @SamyAB on 2024-08-23 09:45_

The documentation says:

> By default, uv's pip interface, i.e., [uv pip compile](https://docs.astral.sh/uv/pip/compile/), produces a resolution that is platform-specific, like pip-tools. There is no way to use platform-specific resolution in the uv's project interface.

Does the last sentence mean that there is _currently_ no way to create a platform-specific lockfile with `uv lock`, or there will _never_ be one?

If it's the latter, I am curious to know why.

The reason I am asking this is because it happens quite often that I work on both `x86` and `armv7l` architectures, and having a lockfile for x86 that points to `pypi`-like registry and a lockfile for `armv7l` that points to a [piwheels](https://www.piwheels.org/)-like registry is very useful.

Thank you for the awesome work!

---

_Comment by @zanieb on 2024-08-23 13:25_

There actually is since https://github.com/astral-sh/uv/pull/6210 — we need to update the documentation to clarify this.

---

_Label `documentation` added by @zanieb on 2024-08-23 13:25_

---

_Label `question` added by @zanieb on 2024-08-23 13:25_

---

_Comment by @SamyAB on 2024-08-23 15:21_

That gets me 95% of the way there, thanks!

---

_Assigned to @zanieb by @zanieb on 2024-08-23 15:27_

---

_Comment by @charliermarsh on 2024-12-26 16:53_

See: https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments

---

_Closed by @charliermarsh on 2024-12-26 16:53_

---
