---
number: 12015
title: "Treat path sources as `package = true` by default"
type: issue
state: closed
author: charliermarsh
labels:
  - breaking
assignees: []
created_at: 2025-03-06T18:20:55Z
updated_at: 2025-07-17T22:20:27Z
url: https://github.com/astral-sh/uv/issues/12015
synced_at: 2026-01-10T01:25:14Z
---

# Treat path sources as `package = true` by default

---

_Issue opened by @charliermarsh on 2025-03-06 18:20_

In `[tool.uv.sources]`, `path` sources should be treated as packages by default (even if they don't specify a build system). It's confusing that we treat them as virtual. As of https://github.com/astral-sh/uv/pull/12014, users can annotate such dependencies with `package = false` as an escape hatch.

See: https://github.com/astral-sh/uv/pull/12014.

See: https://github.com/astral-sh/uv/issues/11998.

---

_Label `breaking` added by @charliermarsh on 2025-03-06 18:21_

---

_Added to milestone `v0.7.0` by @charliermarsh on 2025-03-06 18:21_

---

_Referenced in [astral-sh/uv#11351](../../astral-sh/uv/issues/11351.md) on 2025-04-16 09:02_

---

_Label `needs-design` added by @zanieb on 2025-04-30 17:38_

---

_Comment by @zanieb on 2025-04-30 17:38_

This didn't land in 0.7.0 because we need consensus on a design.

---

_Assigned to @konstin by @zanieb on 2025-04-30 17:38_

---

_Removed from milestone `v0.7.0` by @zanieb on 2025-04-30 17:38_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-30 17:38_

---

_Unassigned @konstin by @konstin on 2025-06-06 11:42_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-12 19:46_

---

_Label `needs-design` removed by @jtfmumm on 2025-07-02 09:25_

---

_Referenced in [astral-sh/uv#14413](../../astral-sh/uv/pulls/14413.md) on 2025-07-02 09:28_

---

_Referenced in [astral-sh/uv#14608](../../astral-sh/uv/issues/14608.md) on 2025-07-14 15:34_

---

_Closed by @zanieb on 2025-07-17 22:20_

---
