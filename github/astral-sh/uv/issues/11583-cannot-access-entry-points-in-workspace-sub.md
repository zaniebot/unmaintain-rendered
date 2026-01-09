---
number: 11583
title: Cannot access entry points in workspace sub packages
type: issue
state: open
author: boccileonardo
labels:
  - needs-mre
assignees: []
created_at: 2025-02-17T20:45:48Z
updated_at: 2025-04-15T11:33:14Z
url: https://github.com/astral-sh/uv/issues/11583
synced_at: 2026-01-07T13:12:18-06:00
---

# Cannot access entry points in workspace sub packages

---

_Issue opened by @boccileonardo on 2025-02-17 20:45_

### Summary

I have a workspace project where I want to make the entry point of a subpackage available.
I tried both referencing it in the pyproject toml via the dot notation, and importing it in the top-level package init. I could not make either option work.
Probably I am misunderstanding something on how I'm supposed to reference these entry points.

MRE:
~$ mkdir uv_pack
~$ cd uv_pack
~/uv_pack $ uv init --package
~/uv_pack $ mkdir packages && mkdir packages/packone && cd packages/packone
~/uv_pack/packages/packone$ uv init --package

```
#~uv_pack/pyproject.toml
...
dependencies = ["packone"]

[tool.uv.sources]
packone = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
...
```

Option 1 (direct reference to entry point in sub package):
```
[project.scripts]
sub-package-entry = "packone:main"
```
Output after installing the wheel into separate env and running uv run sub-package-entry:
ImportError: cannot import name 'main' from 'packone'

Option 2 (importing from subpackage and exposing top-level entry point):
```
#pyproject.toml
[project.scripts]
sub-package-entry = "uv_pack:main_pone"

#src/uv_pack/__init__,py
from packone import main as packone_main
```
Output after installing the wheel into separate env and running uv run sub-package-entry:
ImportError: cannot import name 'main' from 'packone'

### Platform

WSL Ubuntu

### Version

uv 0.6.1

### Python version

3.10.12

---

_Label `bug` added by @boccileonardo on 2025-02-17 20:45_

---

_Comment by @konstin on 2025-02-27 13:56_

I can't reproduce this, for me, it doesn't show `ImportError: cannot import name 'main' from 'packone'` but correctly print "Hello from packone!", no matter where the entrypoint is.

---

_Label `bug` removed by @konstin on 2025-02-27 13:56_

---

_Label `needs-mre` added by @konstin on 2025-02-27 13:56_

---

_Comment by @ghost on 2025-04-02 14:25_

@konstin Hi, sorry for the delay. I retried this on a separate machine (ubuntu instead of wsl). I get the exact same error. When you tried replicating, did you install the built wheel into a separate environment?

I can only successfully call sub-package-entry in the same workspace environment, not when I install the wheel elsewhere.

---

_Comment by @konstin on 2025-04-15 11:33_

> When you tried replicating, did you install the built wheel into a separate environment?

Yes, it worked in a different venv for me

---
