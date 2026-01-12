```yaml
number: 15968
title: "PLE1310 fails on `bytes.strip`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-02-05T13:35:47Z
updated_at: 2025-02-07T20:31:08Z
url: https://github.com/astral-sh/ruff/issues/15968
synced_at: 2026-01-12T15:54:55Z
```

# PLE1310 fails on `bytes.strip`

---

_@dscorbett_

### Description

[`bad-str-strip-call` (PLE1310)](https://docs.astral.sh/ruff/rules/bad-str-strip-call/) in Ruff 0.9.4 flags a `bytes.strip` call when its argument is a `str`, but that is a false positive because `bytes.strip` does not accept a `str` argument.
```console
$ cat >ple1310.py <<'# EOF'
b"".strip("//")
# EOF

$ python ple1310.py 2>&1 | tail -n 1
TypeError: a bytes-like object is required, not 'str'

$ ruff --isolated check --select PLE1310 ple1310.py --output-format concise
ple1310.py:1:11: PLE1310 String `strip` call contains duplicate characters
Found 1 error.
```
PLE1310 does not flag a `bytes.strip` call when its argument is a `bytes`, which a false negative.
```console
$ echo 'b"".strip(b"//")' | ruff --isolated check --select PLE1310 -
All checks passed!
```

---

_Label `bug` added by @dylwil3 on 2025-02-05 13:55_

---

_Label `rule` added by @dylwil3 on 2025-02-05 13:55_

---

_Comment by @InSyncWithFoo on 2025-02-05 19:05_

There seems to be another, escaping-related bug:

```python
''.strip('\\b\\x08')  # No error, even though `\` is repeated twice
```

---

_Closed by @dylwil3 on 2025-02-07 20:31_

---
