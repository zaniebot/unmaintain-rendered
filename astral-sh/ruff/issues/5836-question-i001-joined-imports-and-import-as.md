```yaml
number: 5836
title: "[question] `I001`, joined imports and `import as`"
type: issue
state: closed
author: dorschw
labels:
  - isort
assignees: []
created_at: 2023-07-17T16:05:58Z
updated_at: 2023-07-17T16:55:10Z
url: https://github.com/astral-sh/ruff/issues/5836
synced_at: 2026-01-12T15:54:45Z
```

# [question] `I001`, joined imports and `import as`

---

_@dorschw_

Hi,`I001` suggests
```diff
- from x import a
- from x import b 
+ from x import a, b
```
On the other hand, it doesn't say anything about 
```python
from x import a as aa
from x import b as bb
```

I'd expect it to suggest
```diff
- from x import a as aa
- from x import b as bb
+ from x import (a as aa, b as bb)
```

(real example: https://github.com/demisto/demisto-sdk/pull/3308#discussion_r1265550042, https://github.com/demisto/demisto-sdk/pull/3308#discussion_r1265576685)

Is this by design, or should it suggest grouping even when `as` is used, and this behavior is a bug?

Thanks! 🙂 

---

_Renamed from "I001 and joined imports" to "[question] `I001`, joined imports and `import as`" by @dorschw on 2023-07-17 16:06_

---

_Comment by @charliermarsh on 2023-07-17 16:55_

Check out [`combine-as-imports`](https://beta.ruff.rs/docs/settings/#isort-combine-as-imports), which I think does what you're describing.

---

_Closed by @charliermarsh on 2023-07-17 16:55_

---

_Label `isort` added by @charliermarsh on 2023-07-17 16:55_

---
