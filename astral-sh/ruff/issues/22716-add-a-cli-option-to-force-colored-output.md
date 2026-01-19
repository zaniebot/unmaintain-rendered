```yaml
number: 22716
title: Add a cli option to force colored output
type: issue
state: open
author: lypwig
labels:
  - good first issue
  - cli
assignees: []
created_at: 2026-01-19T11:05:50Z
updated_at: 2026-01-19T15:31:36Z
url: https://github.com/astral-sh/ruff/issues/22716
synced_at: 2026-01-19T16:26:54Z
```

# Add a cli option to force colored output

---

_@lypwig_

It can be required to force the output to be colored.

I know there is [an environment variable](https://docs.astral.sh/ruff/faq/#how-can-i-disableforce-ruffs-color-output) for that, but it's not so practical and [might be tricky](https://github.com/nat-n/poethepoet/issues/351#issuecomment-3767642134) in some cases.

So it can be interesting to have a `--color` cli option [like in ty](https://docs.astral.sh/ty/reference/cli/#ty-check--color).

---

_Comment by @MichaReiser on 2026-01-19 11:23_

Wow, I'm very surprised that Ruff doesn't have this option. We should add it.

---

_Label `good first issue` added by @MichaReiser on 2026-01-19 11:23_

---

_Label `cli` added by @MichaReiser on 2026-01-19 11:23_

---

_Comment by @cwkang1998 on 2026-01-19 15:21_

Would like to try my hand at this

---

_Assigned to @cwkang1998 by @ntBre on 2026-01-19 15:31_

---
