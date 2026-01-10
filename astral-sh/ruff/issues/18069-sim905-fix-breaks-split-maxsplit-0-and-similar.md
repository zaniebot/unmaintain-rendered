```yaml
number: 18069
title: "SIM905 fix breaks `\"\".split(maxsplit=0)` and similar expressions"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-05-13T14:41:27Z
updated_at: 2025-05-14T18:20:19Z
url: https://github.com/astral-sh/ruff/issues/18069
synced_at: 2026-01-10T11:09:58Z
```

# SIM905 fix breaks `"".split(maxsplit=0)` and similar expressions

---

_Issue opened by @dscorbett on 2025-05-13 14:41_

### Summary

The fix for [`split-static-string` (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/) changes the behavior of `str.split` with `sep=None` (the default) and `maxsplit=0` if the string is empty or begins with white space. The output of `split` should not contain an empty string or a string beginning with white space. Similarly, the fix breaks `str.rsplit` if the string is empty or ends with white space.

```console
$ cat >sim905.py <<'# EOF'
print("".split(maxsplit=0))
print(" ".split(maxsplit=0))
print(" x ".split(maxsplit=0))
print("".rsplit(maxsplit=0))
print(" ".rsplit(maxsplit=0))
print(" x ".rsplit(maxsplit=0))
# EOF

$ python sim905.py
[]
[]
['x ']
[]
[]
[' x']

$ ruff --isolated check sim905.py --select SIM905 --fix
Found 6 errors (6 fixed, 0 remaining).

$ python sim905.py
['']
[' ']
[' x ']
['']
[' ']
[' x ']
```

### Version

ruff 0.11.9 (2370297cd 2025-05-09)

---

_Label `bug` added by @ntBre on 2025-05-13 17:48_

---

_Label `fixes` added by @ntBre on 2025-05-13 17:48_

---

_Closed by @ntBre on 2025-05-14 18:20_

---
