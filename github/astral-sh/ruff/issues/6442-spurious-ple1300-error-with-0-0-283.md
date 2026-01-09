---
number: 6442
title: Spurious PLE1300 error with 0.0.283
type: issue
state: closed
author: DimitriPapadopoulos
labels:
  - bug
assignees: []
created_at: 2023-08-09T09:03:25Z
updated_at: 2023-08-22T10:26:21Z
url: https://github.com/astral-sh/ruff/issues/6442
synced_at: 2026-01-07T13:12:15-06:00
---

# Spurious PLE1300 error with 0.0.283

---

_Issue opened by @DimitriPapadopoulos on 2023-08-09 09:03_

~~After upgrading v0.0.281 → v0.0.282 (https://github.com/codespell-project/codespell/pull/3011)~~ we see what we think is a spurious PLE1300 error:

* Minimal code snippet:
  ```python
  s = "{0}{1:{width}}".format("a", "1", width=15)
  ```
* The command invoked:
   ```
   $ ruff --select PLE file.py 
   file.py:1:5: PLE1300 Unsupported format character '}'
   Found 1 error.
   $ 
   ```
* The current Ruff settings: no `pyproject.toml` file
* The current Ruff version:
  ```
  $ ruff --version
  ruff 0.0.283
  $ 
  ```

The [Format String Syntax](https://docs.python.org/3/library/string.html#format-string-syntax) documentation states:
> A _format_spec_ field can also include nested replacement fields within it. These nested replacement fields may contain a field name, conversion flag and format specification, but deeper nesting is not allowed.

---

_Referenced in [codespell-project/codespell#3014](../../codespell-project/codespell/issues/3014.md) on 2023-08-09 09:05_

---

_Comment by @MichaReiser on 2023-08-09 09:08_

This is probably related to #6171

---

_Label `bug` added by @MichaReiser on 2023-08-09 09:08_

---

_Comment by @intgr on 2023-08-09 09:08_

I believe this occurred with the 0.0.282 -> 0.0.283 update, at least that's what caused it for me, not 0.0.281 → 0.0.282.

---

_Comment by @DimitriPapadopoulos on 2023-08-09 09:11_

Perhaps I don't understand how pre-commit checks work. Our `.pre-commit-config.yaml`  contains:
```yaml
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.0.282
    hooks:
      - id: ruff
```

---

_Comment by @DimitriPapadopoulos on 2023-08-09 09:18_

But yes, you're right about it, it's specific to 0.0.283:
```
$ pip install --force-reinstall --user ruff==0.0.281
Collecting ruff==0.0.281
  Downloading ruff-0.0.281-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (5.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.6/5.6 MB 11.0 MB/s eta 0:00:00
Installing collected packages: ruff
  Attempting uninstall: ruff
    Found existing installation: ruff 0.0.283
    Uninstalling ruff-0.0.283:
      Successfully uninstalled ruff-0.0.283
Successfully installed ruff-0.0.281

[notice] A new release of pip is available: 23.1.2 -> 23.2.1
[notice] To update, run: python3 -m pip install --upgrade pip
$ 
$ ruff --select PLE file.py 
$ 
$ pip install --force-reinstall --user ruff==0.0.282
Collecting ruff==0.0.282
  Downloading ruff-0.0.282-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (5.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.6/5.6 MB 12.6 MB/s eta 0:00:00
Installing collected packages: ruff
  Attempting uninstall: ruff
    Found existing installation: ruff 0.0.281
    Uninstalling ruff-0.0.281:
      Successfully uninstalled ruff-0.0.281
Successfully installed ruff-0.0.282

[notice] A new release of pip is available: 23.1.2 -> 23.2.1
[notice] To update, run: python3 -m pip install --upgrade pip
$ 
$ ruff --select PLE file.py 
$ 
$ pip install --force-reinstall --user ruff==0.0.283
Collecting ruff==0.0.283
  Using cached ruff-0.0.283-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (5.7 MB)
Installing collected packages: ruff
  Attempting uninstall: ruff
    Found existing installation: ruff 0.0.282
    Uninstalling ruff-0.0.282:
      Successfully uninstalled ruff-0.0.282
Successfully installed ruff-0.0.283

[notice] A new release of pip is available: 23.1.2 -> 23.2.1
[notice] To update, run: python3 -m pip install --upgrade pip
$ 
$ ruff --select PLE file.py 
file.py:1:5: PLE1300 Unsupported format character '}'
Found 1 error.
$ 
```
I'm not a fan or pre-commit, it keeps annoying me, whether in CI jobs or on my own computer.

---

_Renamed from "Spurious PLE1300 error with 0.0.282" to "Spurious PLE1300 error with 0.0.283" by @DimitriPapadopoulos on 2023-08-09 10:42_

---

_Comment by @kurtmckee on 2023-08-09 12:13_

> pre-commit

ruff v0.0.283 is getting picked up by an unpinned ruff dev dependency running in a separate CI job. pre-commit is running v0.0.282 as specified.

---

_Comment by @charliermarsh on 2023-08-09 15:35_

Thanks! I'll take a look at the format string parser. I wonder if we were silently parsing this incorrectly the whole time, and the rule just brought it to light, or if the changes to the parser from https://github.com/astral-sh/ruff/pull/6171 were erroneous.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-09 20:03_

---

_Comment by @charliermarsh on 2023-08-09 20:46_

I think the parser doesn't quite support this, it'll need to be improved. (The rule was new in v0.0.283.)

---

_Referenced in [astral-sh/ruff#6522](../../astral-sh/ruff/issues/6522.md) on 2023-08-12 13:23_

---

_Assigned to @zanieb by @zanieb on 2023-08-14 17:56_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-14 19:49_

---

_Referenced in [astral-sh/ruff#6616](../../astral-sh/ruff/pulls/6616.md) on 2023-08-16 14:14_

---

_Closed by @zanieb on 2023-08-17 14:07_

---

_Comment by @tdulcet on 2023-08-21 14:27_

This is still broken unfortunately. `test.py`:
```py
import sys

number = 1234.56
print("{0:.{prec}g}".format(number, prec=sys.float_info.dig))
```
Ruff output:
```
$ ruff --select PLE1300 --isolated test.py
test.py:4:7: PLE1300 Unsupported format character 'g'
Found 1 error.
```
Ruff version:
```
$ ruff --version
ruff 0.0.285
```
There is a [full example here](https://gist.github.com/tdulcet/19b89559f6b4f2cc2434547cc7528379#file-gimps_status-py-L336-L366) that triggers two erroneous errors.

---

_Comment by @DimitriPapadopoulos on 2023-08-22 09:57_

@tdulcet Perhaps you should open a new issue, since this one has been automatically closed.

---

_Referenced in [astral-sh/ruff#6767](../../astral-sh/ruff/issues/6767.md) on 2023-08-22 10:19_

---

_Comment by @DimitriPapadopoulos on 2023-08-22 10:20_

I have opened #6767 as a follow-up to this ticket.

---

_Referenced in [astral-sh/ruff#6773](../../astral-sh/ruff/pulls/6773.md) on 2023-08-24 09:07_

---
