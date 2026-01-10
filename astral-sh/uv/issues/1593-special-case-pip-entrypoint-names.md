---
number: 1593
title: Special case pip entrypoint names
type: issue
state: closed
author: woutervh
labels:
  - bug
  - virtualenv
assignees: []
created_at: 2024-02-17T14:28:36Z
updated_at: 2024-03-01T09:05:51Z
url: https://github.com/astral-sh/uv/issues/1593
synced_at: 2026-01-10T01:23:07Z
---

# Special case pip entrypoint names

---

_Issue opened by @woutervh on 2024-02-17 14:28_

on Alma Linux 9.2,  using uv 0.1.3
python3.x  all installed via pyenv

```
 > venv foo -p 3.8 --seed
  Using Python 3.8.18 interpreter at /opt/tools/pyenv/var/repo/versions/3.8.18/bin/python3.8
  Creating virtualenv at: foo
   + setuptools==69.1.0
   + pip==24.0
   + wheel==0.42.0

> ls -al foo/bin
    activate*  
    pip  
    pip3  
    pip3.10      <-- should be pip3.8
    python  
    python3   
    python3.8 
```

the pip / pip3 / pip3.10 are all identical and correct,
so it's just an incorrect filename.


all other versions also create a pip3.10:
```
 > venv foo -p 3.9 --seed
 > venv foo -p 3.11 --seed
 > venv foo -p 3.12 --seed
```




---

_Label `bug` added by @zanieb on 2024-02-17 16:43_

---

_Comment by @zanieb on 2024-02-17 16:44_

Thanks for the report! No idea why this would be.

---

_Comment by @henryiii on 2024-02-17 17:06_

Isn't that `-p`?

---

_Comment by @woutervh on 2024-02-17 18:24_

@henryiii , Yes,  corrected.

---

_Renamed from "venv foo p 3.8 --seed  generates a pip3.10 executable" to "venv foo -p 3.8 --seed  generates a pip3.10 executable" by @woutervh on 2024-02-17 18:27_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Comment by @charliermarsh on 2024-02-20 02:54_

My guess is that it's a caching issue. We cache interpreter metadata using the ctime, to avoid having to run the Python interpreter on every invocation (which is expensive, in a relative sense). Perhaps the ctime isn't changing here?

---

_Comment by @charliermarsh on 2024-02-20 02:55_

Err wait, sorry -- so it creates a file named `pip3.10`, but `pip3.10` is actually `pip3.8`?

---

_Comment by @woutervh on 2024-02-20 17:04_

There is no version info in the pip-entrypoint.

```
> cat pip | pip3 | pip3.10

#!/tmp/foo/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from pip._internal.cli.main import main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(main())
```



---

_Comment by @konstin on 2024-02-20 18:11_

pip has an entrypoint pip3.10 in its wheel: https://files.pythonhosted.org/packages/8a/6a/19e9fe04fca059ccf770861c7d5721ab4c2aebc539889e97c7977528a53b/pip-24.0-py3-none-any.whl. I think you're supposed to special case pip entrypoints (https://github.com/pypa/pip/pull/11547)?

---

_Comment by @konstin on 2024-02-20 18:56_

The pip discord pointed me to https://github.com/pypa/pip/blob/3898741e29b7279e7bffe044ecfbe20f6a438b1e/src/pip/_internal/operations/install/wheel.py#L283, we should special case pip.

---

_Renamed from "venv foo -p 3.8 --seed  generates a pip3.10 executable" to "Special case pip entrypoint names" by @konstin on 2024-02-20 18:56_

---

_Comment by @henryiii on 2024-02-20 18:56_

Better link imo: https://inspector.pypi.io/project/pip/24.0/packages/8a/6a/19e9fe04fca059ccf770861c7d5721ab4c2aebc539889e97c7977528a53b/pip-24.0-py3-none-any.whl/pip-24.0.dist-info/entry_points.txt

---

_Comment by @charliermarsh on 2024-02-20 18:57_

@konstin - Do you mind taking this one?

---

_Assigned to @konstin by @konstin on 2024-02-20 18:58_

---

_Referenced in [astral-sh/uv#1783](../../astral-sh/uv/issues/1783.md) on 2024-02-20 21:17_

---

_Referenced in [pypa/packaging.python.org#1505](../../pypa/packaging.python.org/issues/1505.md) on 2024-02-21 09:59_

---

_Referenced in [astral-sh/uv#1918](../../astral-sh/uv/pulls/1918.md) on 2024-02-23 15:29_

---

_Closed by @konstin on 2024-02-23 18:01_

---

_Comment by @konstin on 2024-02-23 22:50_

Reopening because the upstream fix breaks my workaround: https://github.com/pypa/pip/pull/12536

---

_Reopened by @konstin on 2024-02-23 22:50_

---

_Referenced in [astral-sh/uv#1982](../../astral-sh/uv/pulls/1982.md) on 2024-02-26 13:18_

---

_Closed by @konstin on 2024-03-01 09:05_

---
