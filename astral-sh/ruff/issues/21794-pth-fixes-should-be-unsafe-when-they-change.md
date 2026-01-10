```yaml
number: 21794
title: PTH fixes should be unsafe when they change return types in compound statements
type: issue
state: closed
author: dscorbett
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-12-04T16:22:10Z
updated_at: 2025-12-17T17:31:28Z
url: https://github.com/astral-sh/ruff/issues/21794
synced_at: 2026-01-10T11:10:00Z
```

# PTH fixes should be unsafe when they change return types in compound statements

---

_Issue opened by @dscorbett on 2025-12-04 16:22_

### Summary

The fixes for [`os-rename` (PTH104)](https://docs.astral.sh/ruff/rules/os-rename/), [`os-replace` (PTH105)](https://docs.astral.sh/ruff/rules/os-replace/), [`os-getcwd` (PTH109)](https://docs.astral.sh/ruff/rules/os-getcwd/), and [`os-readlink` (PTH115)](https://docs.astral.sh/ruff/rules/os-readlink/) should be marked unsafe when their values are used in non-expression statements like `if` and `for`. Currently, [`is_top_level_expression_call`](https://github.com/astral-sh/ruff/blob/0.14.8/crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs#L213) only checks for a parent expression, not for a parent statement. [Example](https://play.ruff.rs/6463bd14-0365-4f69-ba97-eac387b01dde):
```console
$ cat >pth1.py <<'# EOF'
import os
import sys

if os.rename("pth1.py", "pth1.py.bak"):
    print("rename: truthy")
else:
    print("rename: falsey")

if os.replace("pth1.py.bak", "pth1.py"):
    print("replace: truthy")
else:
    print("replace: falsey")

try:
    for _ in os.getcwd():
        print("getcwd: iterable")
        break
except TypeError as e:
    print("getcwd: not iterable")
try:
    for _ in os.getcwdb():
        print("getcwdb: iterable")
        break
except TypeError as e:
    print("getcwdb: not iterable")

try:
    for _ in os.readlink(sys.executable):
        print("readlink: iterable")
        break
except TypeError as e:
    print("readlink: not iterable")
# EOF

$ python pth1.py
rename: falsey
replace: falsey
getcwd: iterable
getcwdb: iterable
readlink: iterable

$ ruff --isolated check pth1.py --select PTH104,PTH105,PTH109,PTH115 --preview --fix
Found 5 errors (5 fixed, 0 remaining).

$ python pth1.py
rename: truthy
replace: truthy
getcwd: not iterable
getcwdb: not iterable
readlink: not iterable
```

### Version

ruff 0.14.8 (9d4f1c6ae 2025-12-04)

---

_Label `fixes` added by @ntBre on 2025-12-04 16:27_

---

_Label `preview` added by @ntBre on 2025-12-04 16:27_

---

_Closed by @ntBre on 2025-12-17 17:31_

---
