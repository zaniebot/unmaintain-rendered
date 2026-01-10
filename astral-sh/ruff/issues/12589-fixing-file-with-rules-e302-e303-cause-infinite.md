---
number: 12589
title: Fixing file with rules E302, E303 cause infinite loop
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-07-31T05:05:47Z
updated_at: 2024-08-01T02:09:06Z
url: https://github.com/astral-sh/ruff/issues/12589
synced_at: 2026-01-10T01:22:52Z
---

# Fixing file with rules E302, E303 cause infinite loop

---

_Issue opened by @qarmin on 2024-07-31 05:05_

ruff 0.5.5+369 (138e70bd5 2024-07-30)
```
ruff check *.py --select E302,E303 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
def test_update():
  m=Model(0,0,'a',[])
  # todo(awinter): raw keys, jsonfields
def test_clientmodel():
  "this probably goes away with sync schemas"
```

error
```
/tmp/tmp_folder/data/16997152304333574087.py:5:1: E302 Expected 2 blank lines, found 1
Found 501 errors (500 fixed, 1 remaining).
[*] 1 fixable with the --fix option.

debug error: Failed to converge after 500 iterations in `/tmp/tmp_folder/data/16997152304333574087.py` with rule codes E302:---
def test_update():
  m=Model(0,0,'a',[])

  # todo(awinter): raw keys, jsonfields
def test_clientmodel():
  "this probably goes away with sync schemas"
---
```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16436497/python_compressed.zip)


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 01:44_

---

_Referenced in [astral-sh/ruff#12604](../../astral-sh/ruff/pulls/12604.md) on 2024-08-01 01:45_

---

_Label `bug` added by @charliermarsh on 2024-08-01 01:59_

---

_Label `fuzzer` added by @charliermarsh on 2024-08-01 01:59_

---

_Closed by @charliermarsh on 2024-08-01 02:09_

---

_Closed by @charliermarsh on 2024-08-01 02:09_

---
