```yaml
number: 3438
title: "Allow passing constraint file to `uv pip sync`?"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
assignees: []
created_at: 2024-05-07T20:33:25Z
updated_at: 2024-05-22T16:30:55Z
url: https://github.com/astral-sh/uv/issues/3438
synced_at: 2026-01-10T05:31:37Z
```

# Allow passing constraint file to `uv pip sync`?

---

_Issue opened by @hauntsaninja on 2024-05-07 20:33_

With pip-compile, this can be accomplished via `--pip-args '--constraint c.txt'`.

The use case here is a little niche, since usually you just apply `--constraint` at the time you produce the lock file. But it's possible if your constraints file changes to encounter old lock files (maybe you're going back in history, or you don't automatically keep your lock file up to date), in which case it can be nice to get errors instead of e.g. installing some insecure dependency.

---

_Label `enhancement` added by @charliermarsh on 2024-05-07 23:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-13 02:02_

---

_Comment by @charliermarsh on 2024-05-16 16:49_

I plan to support this but it's proving to be motivation to do some refactoring / cleanup work between `pip sync` and `pip install`, so want to knock that out first.

---

_Closed by @charliermarsh on 2024-05-22 16:30_

---
