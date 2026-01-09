---
number: 8798
title: add rule for none variable
type: issue
state: closed
author: tooptoop4
labels:
  - question
assignees: []
created_at: 2023-11-20T23:09:38Z
updated_at: 2023-11-22T17:02:42Z
url: https://github.com/astral-sh/ruff/issues/8798
synced_at: 2026-01-07T13:12:15-06:00
---

# add rule for none variable

---

_Issue opened by @tooptoop4 on 2023-11-20 23:09_

below is not being flagged by any rule, i would expect it to suggest to flip the order of the conditions
```
try:
    return file_path.rsplit('/', 1)[1].startswith('redact') or not file_path
except Exception:
    return False
```

---

_Comment by @charliermarsh on 2023-11-21 01:36_

Can you share a bit more on why you'd expect Ruff to suggest flipping the order?

---

_Label `question` added by @charliermarsh on 2023-11-21 01:36_

---

_Comment by @tmke8 on 2023-11-21 13:59_

I assume it's because if `file_path` is `None`, then the evaluation of the first condition will throw a runtime error. But to catch this kind of mistake, ruff would need to know the type of `file_path`, so this seems more like a job for a static type checker.

---

_Comment by @Skylion007 on 2023-11-22 15:36_

This feels more under the scope of `type inference`? I think mypy can complain about this with the right settings.

---

_Comment by @Avasam on 2023-11-22 16:16_

Yeah that's more of a type-checker thing.
Although even if it's not `None` it might be generally agreeable to check for truthyness first.

---

_Comment by @charliermarsh on 2023-11-22 17:02_

I'm gonna close for now as this seems more an issue for a type checker. Thanks for filing!

---

_Closed by @charliermarsh on 2023-11-22 17:02_

---
