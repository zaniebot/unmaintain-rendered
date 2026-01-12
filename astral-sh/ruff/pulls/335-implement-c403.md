```yaml
number: 335
title: Implement C403
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: C403
created_at: 2022-10-06T15:17:58Z
updated_at: 2022-10-06T20:24:24Z
url: https://github.com/astral-sh/ruff/pull/335
synced_at: 2026-01-12T05:48:45Z
```

# Implement C403

---

_Pull request opened by @harupy on 2022-10-06 15:17_

#305


```
> cargo run -- --select=C403 resources/test/fixtures/C403.py
resources/test/fixtures/C403.py:1:5: C403 Unnecessary list comprehension - rewrite as a set comprehension
```

---

_Comment by @harupy on 2022-10-06 15:29_

It looks like this check is auto-fixable. I will file a follow-up PR (if this is really auto-fixable).

---

_Comment by @charliermarsh on 2022-10-06 20:23_

This looks great, thanks.

---

_@charliermarsh reviewed on 2022-10-06 20:23_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:772 on 2022-10-06 20:23_

I'm trying to establish a pattern whereby we add new checks to `plugins`, and every plugin takes `&mut self` as its first argument. But that might be overkill for these really simple cases. (Better when there's a lot of logic around the `checks::` call.)

---

_Merged by @charliermarsh on 2022-10-06 20:24_

---

_Closed by @charliermarsh on 2022-10-06 20:24_

---
