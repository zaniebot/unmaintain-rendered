---
number: 11198
title: "Improve `uv pip install` error when `--extra` is used"
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-02-03T22:14:07Z
updated_at: 2025-02-03T22:14:17Z
url: https://github.com/astral-sh/uv/issues/11198
synced_at: 2026-01-07T13:12:18-06:00
---

# Improve `uv pip install` error when `--extra` is used

---

_Issue opened by @zanieb on 2025-02-03 22:14_

> it'd be nice if we could "rewrite" the cli command they passed (or at least substitute the extras into the syntax)

_Originally posted by @Gankra in https://github.com/astral-sh/uv/pull/11193#pullrequestreview-2591162043_

Maybe easier if we special-case a singleton? Problematic with `--all-extras` too, since there's not an equivalent `[...]` selector for that.

---

_Label `error messages` added by @zanieb on 2025-02-03 22:14_

---

_Referenced in [astral-sh/uv#14558](../../astral-sh/uv/issues/14558.md) on 2025-07-11 12:53_

---
