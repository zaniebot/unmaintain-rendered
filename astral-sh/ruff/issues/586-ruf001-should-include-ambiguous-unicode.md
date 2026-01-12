```yaml
number: 586
title: RUF001 should include ambiguous unicode characters in comments
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-04T16:09:37Z
updated_at: 2022-11-08T02:16:35Z
url: https://github.com/astral-sh/ruff/issues/586
synced_at: 2026-01-12T15:54:40Z
```

# RUF001 should include ambiguous unicode characters in comments

---

_@charliermarsh_

Right now, the RustPython tokenizer doesn't include comments (unlike the CPython tokenizer), which is blocking this check.


---

_Label `enhancement` added by @charliermarsh on 2022-11-04 16:09_

---

_Renamed from "X001 should include ambiguous unicode characters in comments" to "RUF001 should include ambiguous unicode characters in comments" by @charliermarsh on 2022-11-05 18:14_

---

_Comment by @charliermarsh on 2022-11-07 02:16_

https://github.com/RustPython/RustPython/pull/4266

---

_Closed by @charliermarsh on 2022-11-08 02:16_

---
