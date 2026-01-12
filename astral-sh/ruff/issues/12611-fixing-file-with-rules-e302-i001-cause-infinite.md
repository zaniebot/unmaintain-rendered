```yaml
number: 12611
title: Fixing file with rules E302, I001 cause infinite loop
type: issue
state: open
author: qarmin
labels:
  - bug
  - help wanted
  - fuzzer
assignees: []
created_at: 2024-08-01T12:45:28Z
updated_at: 2025-10-14T07:24:02Z
url: https://github.com/astral-sh/ruff/issues/12611
synced_at: 2026-01-12T15:54:52Z
```

# Fixing file with rules E302, I001 cause infinite loop

---

_@qarmin_

ruff 0.5.5+379 (a3e67abf4 2024-07-31)
```
ruff check *.py --select E302,I001 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
import time
#替换环境

#测试环境，替换eb部分3台服务器及其代码
def web_replace():
    bg=time.time()
```

error
```
/tmp/tmp_folder/data/5053444380204826248.py:1:1: I001 Import block is un-sorted or un-formatted
Found 501 errors (500 fixed, 1 remaining).
[*] 1 fixable with the --fix option.

debug error: Failed to converge after 500 iterations in `/tmp/tmp_folder/data/5053444380204826248.py` with rule codes I001:---
import time


#替换环境

#测试环境，替换eb部分3台服务器及其代码
def web_replace():
    bg=time.time()
---
```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16456600/python_compressed.zip)



---

_Label `bug` added by @charliermarsh on 2024-08-01 12:46_

---

_Label `fuzzer` added by @charliermarsh on 2024-08-01 12:46_

---

_Comment by @MichaReiser on 2024-08-01 21:24_

Huh, that's funny. Thanks for reporting!

---

_Label `help wanted` added by @MichaReiser on 2024-08-01 21:24_

---

_Comment by @TaKO8Ki on 2025-09-26 07:28_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-26 15:31_

---

_Comment by @MichaReiser on 2025-10-14 07:24_

See https://github.com/astral-sh/ruff/issues/20853#issuecomment-3400349146 for an ASCII-only reproduction and a fix suggestion

---
