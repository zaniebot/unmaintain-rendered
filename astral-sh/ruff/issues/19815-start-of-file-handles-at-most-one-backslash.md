```yaml
number: 19815
title: "`start_of_file` handles at most one backslash, making I002 introduce a syntax error"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-07T20:50:30Z
updated_at: 2025-08-15T21:17:51Z
url: https://github.com/astral-sh/ruff/issues/19815
synced_at: 2026-01-12T15:54:57Z
```

# `start_of_file` handles at most one backslash, making I002 introduce a syntax error

---

_@dscorbett_

### Summary

#19505 made `start_of_file` handle a backslash after a docstring, but it only handles _one_ backslash. If the next line has a backslash too, the original bug appears. [Example](https://play.ruff.rs/79771e2f-e2a0-4751-95c6-8e291c925a04):
```console
$ cat >i002.py <<'# EOF'
"docstring"\
\

print(f"{__doc__=}")
# EOF

$ ruff --isolated check i002.py --select I002 --config 'lint.isort.required-imports=["import sys"]' --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `bug` added by @ntBre on 2025-08-07 21:23_

---

_Label `fixes` added by @ntBre on 2025-08-07 21:23_

---

_Closed by @ntBre on 2025-08-15 21:17_

---
