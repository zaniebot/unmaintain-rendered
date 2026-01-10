```yaml
number: 354
title: Do not exit with status code 1 for IO errors
type: issue
state: closed
author: sharkdp
labels:
  - cli
assignees: []
created_at: 2025-05-13T12:23:38Z
updated_at: 2025-11-19T08:39:20Z
url: https://github.com/astral-sh/ty/issues/354
synced_at: 2026-01-10T01:58:59Z
```

# Do not exit with status code 1 for IO errors

---

_Issue opened by @sharkdp on 2025-05-13 12:23_

We currently exit with code 1 in cases of IO errors:

```
▶ ty check non-existent-file.py
WARN No python files found under the given path(s)
error[io]: `/home/shark/non-existent-file.py`: No such file or directory (os error 2)

Found 1 diagnostic

▶ echo $?
1
```

This is undesirable because it mixes actual diagnostics in the users code (type-checking problems) with problems when performing the check. It is useful for users to be able to distinguish these two cases.

Similarly, as @MichaReiser pointed out in a conversation, it is unlikely that someone would want to suppress IO errors using `--exit-zero`, but that is exactly what happens right now:
```
▶ ty check non-existent-file.py --exit-zero 
WARN No python files found under the given path(s)
error[io]: `/home/shark/non-existent-file.py`: No such file or directory (os error 2)

Found 1 diagnostic

▶ echo $?
0
```

---

_Label `cli` added by @sharkdp on 2025-05-13 12:23_

---

_Closed by @MichaReiser on 2025-11-19 08:39_

---
