```yaml
number: 9646
title: Is it possible to suppress a rule on a block of code?
type: issue
state: closed
author: DaniBodor
labels:
  - suppression
assignees: []
created_at: 2024-01-26T09:52:18Z
updated_at: 2024-01-26T16:39:59Z
url: https://github.com/astral-sh/ruff/issues/9646
synced_at: 2026-01-12T15:54:49Z
```

# Is it possible to suppress a rule on a block of code?

---

_@DaniBodor_

I would like to disable/suppress ruff (or certain rules of ruff) on a block of code. I know that I can do this for single lines (by using # noqa: <rule_code> at the end of the line) or for entire files/folder (by specifying them in the settings file).

However, I would like to disable ruff on an entire function or on a multi-line block of code, but not an entire file. Is there a way to do this, without adding the noqa to the end of each line?

Thanks

---

_Comment by @charliermarsh on 2024-01-26 16:39_

Unfortunately we don't support block-level suppression right now. I believe this is similar to #3711 so I'm going to merge into that issue.

---

_Closed by @charliermarsh on 2024-01-26 16:39_

---

_Label `noqa` added by @charliermarsh on 2024-01-26 16:39_

---
