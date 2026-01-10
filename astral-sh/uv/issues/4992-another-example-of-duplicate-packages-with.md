---
number: 4992
title: "Another example of duplicate packages with different versions using `--universal`"
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-11T14:53:40Z
updated_at: 2024-08-10T01:12:04Z
url: https://github.com/astral-sh/uv/issues/4992
synced_at: 2026-01-10T01:23:44Z
---

# Another example of duplicate packages with different versions using `--universal`

---

_Issue opened by @notatallshaw-gts on 2024-07-11 14:53_

```
$ uv -V
uv 0.2.24

$ echo -e "pylint>=2.14.5\ndill==0.3.1.1" | uv pip compile --python-version 3.10.6 --universal --annotation-style line - | grep "pylint=="
Resolved 14 packages in 43ms
pylint==2.15.8
pylint==3.2.5
```

My original example had the dill requirement passed in via a constraint, but after cutting out hundreds of requirements and constraints was able to reproduce a minimal example with just requirements.

---

_Label `bug` added by @konstin on 2024-07-11 14:59_

---

_Label `preview` added by @konstin on 2024-07-11 14:59_

---

_Comment by @konstin on 2024-07-11 15:00_

Thanks for the clear reproduction!

`pylint==3.25` has:
```
Requires-Dist: dill >=0.2 ; python_version < "3.11"
Requires-Dist: dill >=0.3.6 ; python_version >= "3.11"
Requires-Dist: dill >=0.3.7 ; python_version >= "3.12"
```

We're solving once for `python_version >= '3.12'` and once for `python_version < '3.11'`, there's at least some part of #4732 here.

---

_Renamed from "Another example of duplicate lines with `--universal`" to "Another example of duplicate packages with different versions using `--universal`" by @notatallshaw-gts on 2024-07-11 15:04_

---

_Referenced in [astral-sh/uv#4996](../../astral-sh/uv/pulls/4996.md) on 2024-07-11 18:07_

---

_Comment by @notatallshaw-gts on 2024-08-10 01:11_

This appears to be solved in 0.2.35 (https://github.com/astral-sh/uv/pull/5898?) I now get:

```
pylint==2.15.8 ; python_version >= '3.11'
pylint==3.2.6 ; python_version < '3.11'
```

---

_Closed by @notatallshaw-gts on 2024-08-10 01:12_

---
