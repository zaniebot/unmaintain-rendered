```yaml
number: 736
title: "`I001` raised on comments in import section"
type: issue
state: closed
author: lolrenx
labels:
  - bug
assignees: []
created_at: 2022-11-14T14:47:01Z
updated_at: 2022-11-14T14:59:29Z
url: https://github.com/astral-sh/ruff/issues/736
synced_at: 2026-01-12T15:54:40Z
```

# `I001` raised on comments in import section

---

_@lolrenx_

raised on inline / single line comments
not sure if this is intended. do commentaries in imports violate a PEP rule ? 


---

_Renamed from "any commentary (inline or single line) in imports is removed" to "any commentary (inline or single line) in imports is removed when using `I` rules" by @lolrenx on 2022-11-14 14:47_

---

_Renamed from "any commentary (inline or single line) in imports is removed when using `I` rules" to "`I001` raised on comments in import section" by @lolrenx on 2022-11-14 14:50_

---

_Comment by @charliermarsh on 2022-11-14 14:53_

Yeah it’s probably due to comments right now! This will be fixed soon (see #675).

---

_Label `bug` added by @charliermarsh on 2022-11-14 14:54_

---

_Comment by @lolrenx on 2022-11-14 14:58_

awesome! cheers. really sorry for the duplicate, I did search thought!

---

_Comment by @lolrenx on 2022-11-14 14:59_

https://github.com/charliermarsh/ruff/issues/675

---

_Closed by @lolrenx on 2022-11-14 14:59_

---
