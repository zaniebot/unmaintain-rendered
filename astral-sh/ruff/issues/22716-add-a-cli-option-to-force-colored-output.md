```yaml
number: 22716
title: Add a cli option to force colored output
type: issue
state: open
author: lypwig
labels: []
assignees: []
created_at: 2026-01-19T11:05:50Z
updated_at: 2026-01-19T11:05:50Z
url: https://github.com/astral-sh/ruff/issues/22716
synced_at: 2026-01-19T11:30:15Z
```

# Add a cli option to force colored output

---

_@lypwig_

It can be required to force the output to be colored.

I know there is [an environment variable](https://docs.astral.sh/ruff/faq/#how-can-i-disableforce-ruffs-color-output) for that, but it's not so practical and [might be tricky](https://github.com/nat-n/poethepoet/issues/351#issuecomment-3767642134) in some cases.

So it can be interesting to have a `--color` cli option [like in ty](https://docs.astral.sh/ty/reference/cli/#ty-check--color).

---
