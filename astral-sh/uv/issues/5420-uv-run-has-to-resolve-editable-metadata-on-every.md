---
number: 5420
title: "`uv run` has to resolve editable metadata on every invocation"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-24T20:07:37Z
updated_at: 2024-07-25T13:45:53Z
url: https://github.com/astral-sh/uv/issues/5420
synced_at: 2026-01-10T01:23:48Z
---

# `uv run` has to resolve editable metadata on every invocation

---

_Issue opened by @charliermarsh on 2024-07-24 20:07_

See: https://github.com/astral-sh/uv/issues/4946#issuecomment-2248789268.

I didn't notice this because most of the projects we test with have static metadata, which is ~free. But, when we resolve, even from a lockfile, we start with a path to an editable directory (the project root). We need to resolve, then install the editable. To resolve an editable without static metadata, we have to invoke Python, and we actually _don't_ cache that metadata at all (even though we accept a stale editable as "satisfying" an install request).


---

_Label `bug` added by @charliermarsh on 2024-07-24 20:07_

---

_Label `preview` added by @charliermarsh on 2024-07-24 20:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 20:07_

---

_Referenced in [astral-sh/uv#5423](../../astral-sh/uv/pulls/5423.md) on 2024-07-24 20:23_

---

_Closed by @charliermarsh on 2024-07-25 13:45_

---
