---
number: 7207
title: "UV doesn't find new package from PyPI"
type: issue
state: closed
author: JoshuaPurtell
labels:
  - question
assignees: []
created_at: 2024-09-09T00:29:10Z
updated_at: 2025-11-25T10:41:16Z
url: https://github.com/astral-sh/uv/issues/7207
synced_at: 2026-01-10T01:24:12Z
---

# UV doesn't find new package from PyPI

---

_Issue opened by @JoshuaPurtell on 2024-09-09 00:29_

I maintain a few packages mostly for personal use.

After upload to pypi via 
```
pip install build twine
python -m build
twine upload dist/*
```

I can immediately download the package via pip like
```
pip install package-name
```

However, it takes between 2 and 45 minutes to be able to successfully run

```
uv pip install package-name
```

Sometimes restarting my computer appears to help, which may relate to some cacheing?


---

_Comment by @charliermarsh on 2024-09-09 00:36_

Does adding `--refresh` help? We cache PyPI responses for 10 minutes per the HTTP caching headers.

---

_Label `question` added by @charliermarsh on 2024-09-09 00:36_

---

_Closed by @charliermarsh on 2024-09-14 00:43_

---

_Comment by @surister on 2025-04-05 14:36_

Solution: `uv add --refresh cratedb-async==0.0.6`

---

_Comment by @zanieb on 2025-04-05 16:05_

You can also `--refresh-package <name>` to avoid refreshing other cache entries.

---

_Comment by @alelom on 2025-11-24 17:06_

Only `--refresh-package package-name` worked for me to refresh the actual cached package. The general `refresh` didn't work in gathering the latest upload from the PyPI.

E.g.

```bash
uv add python-package-folder==1.4.0 --group build --refresh-package python-package-folder
```

Worked before the 10 minutes cache time, whilst:

```bash
uv add python-package-folder==1.4.0 --group build --refresh
```

didn't.

---
