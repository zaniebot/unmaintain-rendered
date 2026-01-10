```yaml
number: 11701
title: Failed to parse uv.lock file
type: issue
state: closed
author: AnanyaRaval
labels:
  - question
assignees: []
created_at: 2025-02-21T19:10:44Z
updated_at: 2025-07-23T19:09:15Z
url: https://github.com/astral-sh/uv/issues/11701
synced_at: 2026-01-10T03:32:45Z
```

# Failed to parse uv.lock file

---

_Issue opened by @AnanyaRaval on 2025-02-21 19:10_

### Question

##### Summary
I am getting the following error while using `uv run file.py`  - 
```bash
error: Failed to parse `uv.lock`
  Caused by: Dependency `numpy` has a missing `source` field but has more than one matching package. 
```
Manually searching the file, I see only one place where `numpy` is mentioned without `source` field. This package was added before multiple versions of numpy were introduced in the project.

```
[[package]]
name = "tableone"
version = "0.9.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "jinja2" },
    { name = "numpy" },
    { name = "openpyxl" },
    { name = "pandas" },
    { name = "scipy" },
    { name = "statsmodels" },
    { name = "tabulate" },
]
```

#### Context
It occurred after I pulled the latest `uv.lock` file from remote repository. It was pushed after running `uv pip install scikit-survival` (`uv add` was throwing an error for not finding `numpy` even when it was installed). I'd appreciate any help in fixing this issue. 

Attaching `pyproject.toml` and `uv.lock` files. 

[uv.lock.txt](https://github.com/user-attachments/files/18914154/uv.lock.txt)
[pyproject.toml.txt](https://github.com/user-attachments/files/18914171/pyproject.toml.txt)

 

### Platform

Ubuntu 18.04.6 LTS

### Version

uv 0.6.0

---

_Label `question` added by @AnanyaRaval on 2025-02-21 19:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-01 00:32_

---

_Comment by @charliermarsh on 2025-03-01 01:45_

Hmm. I can reproduce this by running `uv lock`, but if I `rm uv.lock`, `uv lock` succeeds -- so I can't reproduce it "from scratch". Is there any chance there was a merge or rebase conflict that was resolved incorrectly on the lockfile?

---

_Closed by @charliermarsh on 2025-03-14 00:39_

---

_Comment by @charliermarsh on 2025-03-14 00:39_

Closing due to lack of follow-up, but happy to help if this gets revived.

---

_Comment by @gnowland on 2025-07-23 19:08_

Running `rm uv.lock` and `uv lock` resolved this for me. BTW running `uv lock --refresh` worked on one machine but not another, so the more robust solution is to nuke the lockfile and recreate from scratch (as is often the case when working with cache!)

---
