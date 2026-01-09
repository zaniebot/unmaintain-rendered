---
number: 11488
title: "`uv add --editable .` package isn't recognized by static analysis"
type: issue
state: closed
author: tonydavis629
labels:
  - question
  - external
assignees: []
created_at: 2025-02-13T19:02:51Z
updated_at: 2025-02-13T20:39:06Z
url: https://github.com/astral-sh/uv/issues/11488
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv add --editable .` package isn't recognized by static analysis

---

_Issue opened by @tonydavis629 on 2025-02-13 19:02_

### Summary

Following this issue https://github.com/astral-sh/uv/issues/3898, I'm trying to understand how to make pylance see local packages installed with `uv add --editable`. The package itself works fine, it's just the vscode/pylance static analysis that cannot find the package.

I have the current setting in my pyproject.toml:
```
[tool.uv]
config-settings = { editable_mode = "compat" }
```

I also tried including that setting in the package's pyproject.toml to no effect. 

I understand there's some turmoil generally caused by PEP660 https://github.com/microsoft/pylance-release/issues/3473 but as far as I understand uv is supposed to be PEP660 compatible? Is this still an  upstream issue? Or am I doing something wrong?

### Platform

Linux 5.14.0-427.33.1.el9_4.x86_64 x86_64 GNU/Linux

### Version

uv 0.5.31

### Python version

python 3.12.7

---

_Label `bug` added by @tonydavis629 on 2025-02-13 19:02_

---

_Comment by @konstin on 2025-02-13 19:16_

PEP 660 is very unopinionated about how the editable installation works. uv is PEP 660 compatible, it installs whatever the build backend gives it, and this layout is a decision made by setuptools (https://github.com/pypa/setuptools/issues/3518). uv can't really do anything here, it has to respect what the build backend does. One option is to switch to different build backend with a different editable behavior.

---

_Label `bug` removed by @konstin on 2025-02-13 19:16_

---

_Label `question` added by @konstin on 2025-02-13 19:16_

---

_Label `upstream` added by @konstin on 2025-02-13 19:16_

---

_Comment by @zanieb on 2025-02-13 19:18_

See also https://github.com/astral-sh/uv/issues/10946#issuecomment-2613366284

---

_Comment by @zanieb on 2025-02-13 19:34_

I am surprised this isn't working with the compat mode though.

---

_Comment by @charliermarsh on 2025-02-13 19:36_

You might just need to restart Pylance or something after installing.

---

_Comment by @tonydavis629 on 2025-02-13 20:37_

I'm not sure what I did but pylance started to find it suddenly after I edited my package. I'm pretty confused, but it does seem to work, just maybe not super reliably. 

> You might just need to restart Pylance or something after installing.

Yeah I tried that initially but didn't help. 

---

_Closed by @tonydavis629 on 2025-02-13 20:37_

---
