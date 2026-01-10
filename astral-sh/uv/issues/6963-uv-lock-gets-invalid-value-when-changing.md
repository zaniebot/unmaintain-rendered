---
number: 6963
title: uv.lock gets invalid value when changing pyproject.toml manually
type: issue
state: closed
author: Morgadoooo
labels:
  - needs-mre
assignees: []
created_at: 2024-09-03T09:49:00Z
updated_at: 2024-09-05T02:09:24Z
url: https://github.com/astral-sh/uv/issues/6963
synced_at: 2026-01-10T01:24:08Z
---

# uv.lock gets invalid value when changing pyproject.toml manually

---

_Issue opened by @Morgadoooo on 2024-09-03 09:49_

Hi, 

When using `pyproject.toml` for managing dependencies, I got a weird behaviour. 
Using `python 3.9.16` and `uv 0.4.2`
My first `pyproject.toml` is this one
```toml
[project]
name = "toto"
version = "1.0.0"
requires-python = ">=3.8,<3.13"
dependencies = [
  "numpy>=1.18.5",
  "pandas>=1.1.5",
]

....
```
When i do `uv lock` everything is fine and I also can do `uv sync`
I noticed that for python 3.12 you cannot use numpy <1.26.4, so I decided to update the pyproject.toml manually like this.
```toml
[project]
name = "toto"
version = "1.0.0"
requires-python = ">=3.8,<3.13"
dependencies = [
  "numpy>=1.18.5; python_version<'3.12'",
  "numpy>1.26.4; python_version=='3.12'",
  "pandas>=1.1.5; python_version<'3.12'",
  "pandas>=2.1.0; python_version=='3.12'",
]

...
```
But now if I try to do `uv lock` and then `uv sync` in python 3.9.16 uv tries to install numpy==2.1.0 which is only available for python >= 3.10

Deleting the `uv.lock` and doing again `uv lock` fixes the issue

---

_Comment by @charliermarsh on 2024-09-03 12:55_

I haven't been able to reproduce this yet:

```
❯ uv sync --python 3.9.16
Using Python 3.9.16
Creating virtualenv at: .venv
Resolved 9 packages in 0.62ms
Prepared 6 packages in 825ms
Installed 6 packages in 58ms
 + numpy==1.24.4
 + pandas==2.0.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + six==1.16.0
 + tzdata==2024.1
```

---

_Label `needs-mre` added by @charliermarsh on 2024-09-03 12:55_

---

_Comment by @charliermarsh on 2024-09-03 12:55_

Any idea what I could be doing wrong? I just took the first `pyproject.toml`, ran `uv lock`, then replaced it with the second, ran `uv lock`, then ran `uv sync --python 3.9.16`.

---

_Comment by @Morgadoooo on 2024-09-03 13:04_

Did you do the first `uv sync` before changing the pyproject ?
For full context on the steps, 
```bash
uv venv -p 3.9
# python 3.9.16 used here
uv lock
uv sync

# I change the pyproject.toml manually

uv lock
uv sync
```

---

_Comment by @charliermarsh on 2024-09-03 13:45_

It's still looking right to me, hmm:

```console
❯ uv lock -p 3.9.16
Using Python 3.9.16
Resolved 7 packages in 8ms

❯ uv sync -p 3.9.16
Using Python 3.9.16
Creating virtualenv at: .venv
Resolved 7 packages in 0.38ms
Prepared 6 packages in 0.29ms
Installed 6 packages in 33ms
 + numpy==1.24.4
 + pandas==2.0.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + six==1.16.0
 + tzdata==2024.1

❯ uv lock -p 3.9.16
Resolved 9 packages in 4ms
Updated numpy v1.24.4 -> v1.24.4, v2.1.0
Updated pandas v2.0.3 -> v2.0.3, v2.2.2

❯ uv sync -p 3.9.16
Resolved 9 packages in 0.81ms
Audited 6 packages in 0.02ms

❯ uv pip list
Package         Version
--------------- -----------
numpy           1.24.4
pandas          2.0.3
python-dateutil 2.9.0.post0
pytz            2024.1
six             1.16.0
tzdata          2024.1
```

---

_Comment by @Morgadoooo on 2024-09-03 14:07_

Ok so, thanks to you I think I got where the issue came from. 

I had a dependency in my `[tool.uv] dev-dependencies` that also had deps for numpy and pandas, but this package had his deps like this
```toml
requires-python = ">=3.8"
dependencies = [
  "numpy>=1.18.5",
  "pandas>=1.1.5",
]
```
if i remove the package from the dev-dependencies there is no problem. 

The package is a private package so I can totally fix it on my own to prevent this issue by changing the deps of the external package.

 

---

_Comment by @Morgadoooo on 2024-09-03 14:14_

For context too, the external package was built using poetry and 
```toml
[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

I don't know if it's relevant or not just to give more info on the package that was giving the issue here

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-03 17:10_

---

_Comment by @charliermarsh on 2024-09-03 17:31_

Interesting... I would still expect it to avoid choosing a version that's incompatible with Python 3.9. Happy to look into it further if you can provide a full reproduction, otherwise I'll close out for now.

---

_Closed by @charliermarsh on 2024-09-05 02:09_

---
