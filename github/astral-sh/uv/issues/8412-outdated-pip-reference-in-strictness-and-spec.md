---
number: 8412
title: "Outdated pip reference in \"Strictness and spec enforcement\" docs"
type: issue
state: closed
author: edmorley
labels:
  - documentation
assignees: []
created_at: 2024-10-21T09:40:38Z
updated_at: 2024-11-05T02:32:47Z
url: https://github.com/astral-sh/uv/issues/8412
synced_at: 2026-01-07T13:12:17-06:00
---

# Outdated pip reference in "Strictness and spec enforcement" docs

---

_Issue opened by @edmorley on 2024-10-21 09:40_

On https://docs.astral.sh/uv/pip/compatibility/#strictness-and-spec-enforcement it says:

> uv tends to be stricter than pip, and will often reject packages that pip would install. For example, uv omits packages with invalid version specifiers in its metadata, which pip similarly plans to exclude in a [future release](https://github.com/pypa/pip/issues/12063).

As of pip 24.1, pip now rejects those versions too - so it's no longer a "future release".

I was going to open a PR to remove that last sentence, however, it seems that it would be useful if this section of the docs still had an example - so I thought you might want to add another in its place (and I don't know of any)?

---

_Comment by @zanieb on 2024-10-21 13:45_

Thanks! I guess we could say "For example, uv omits packages with invalid version specifiers in its metadata, which pip similarly excludes in recent versions" and punt another example to the future?

@konstin may have another example.

---

_Label `documentation` added by @zanieb on 2024-10-21 13:45_

---

_Comment by @konstin on 2024-10-22 09:52_

Pre-PEP 517 backwards is stronger in pip, we could use that as new example.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-05 01:57_

---

_Comment by @charliermarsh on 2024-11-05 01:57_

I found something to replace this with.

---

_Referenced in [astral-sh/uv#8822](../../astral-sh/uv/pulls/8822.md) on 2024-11-05 02:30_

---

_Closed by @charliermarsh on 2024-11-05 02:32_

---
