---
number: 16525
title: S324 false negative on mixed-case hash algorithm names
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-03-05T20:37:05Z
updated_at: 2025-03-07T17:54:03Z
url: https://github.com/astral-sh/ruff/issues/16525
synced_at: 2026-01-07T13:12:16-06:00
---

# S324 false negative on mixed-case hash algorithm names

---

_Issue opened by @dscorbett on 2025-03-05 20:37_

### Summary

`hashlib.new` interprets the algorithm name case-insensitively. [`hashlib-insecure-hash-function` (S324)](https://docs.astral.sh/ruff/rules/hashlib-insecure-hash-function/) recognizes lowercase and uppercase names, but not mixed-case names.
```console
$ cat >s324.py <<'# EOF'
import hashlib
print(hashlib.new("Md5").hexdigest())
# EOF

$ python s324.py
d41d8cd98f00b204e9800998ecf8427e

$ ruff check --isolated --select S324 s324.py
All checks passed!
```

### Version

_No response_

---

_Label `bug` added by @ntBre on 2025-03-05 20:45_

---

_Label `help wanted` added by @ntBre on 2025-03-05 20:46_

---

_Label `good first issue` added by @MichaReiser on 2025-03-06 07:23_

---

_Comment by @VascoSch92 on 2025-03-07 11:15_

If no one is taking this, I can do it. It should be quick! 😉

---

_Assigned to @VascoSch92 by @MichaReiser on 2025-03-07 11:31_

---

_Referenced in [astral-sh/ruff#16552](../../astral-sh/ruff/pulls/16552.md) on 2025-03-07 13:22_

---

_Comment by @ntBre on 2025-03-07 17:54_

I think this was closed by #16552 🎉 

---

_Closed by @ntBre on 2025-03-07 17:54_

---
