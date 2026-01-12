```yaml
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
synced_at: 2026-01-12T16:00:30Z
```

# Improve `uv pip install` error when `--extra` is used

---

_@zanieb_

> it'd be nice if we could "rewrite" the cli command they passed (or at least substitute the extras into the syntax)

_Originally posted by @Gankra in https://github.com/astral-sh/uv/pull/11193#pullrequestreview-2591162043_

Maybe easier if we special-case a singleton? Problematic with `--all-extras` too, since there's not an equivalent `[...]` selector for that.

---

_Label `error messages` added by @zanieb on 2025-02-03 22:14_

---
