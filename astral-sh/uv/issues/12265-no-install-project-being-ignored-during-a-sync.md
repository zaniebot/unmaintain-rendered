```yaml
number: 12265
title: "`--no-install-project` being ignored during a sync"
type: issue
state: closed
author: VERBOSE-01
labels:
  - needs-mre
assignees: []
created_at: 2025-03-18T09:07:26Z
updated_at: 2025-03-24T15:18:30Z
url: https://github.com/astral-sh/uv/issues/12265
synced_at: 2026-01-12T16:00:59Z
```

# `--no-install-project` being ignored during a sync

---

_@VERBOSE-01_

### Question

I'm trying a [partial installation](https://docs.astral.sh/uv/concepts/projects/sync/#partial-installations) (all dependencies, except my project) using the `--no-install-project` flag but unfortunately, `uv` still tries to install the project itself.

#### Tree
```
> my_personal_project
--> # src folders and files
--> pyproject.toml
--> .python-version
```

#### Terminal

```bash
PS C:\Users\verbose\Desktop\my_personal_project> uv sync --no-install-project
Using CPython 3.13.2 interpreter at: C:\Users\verbose\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment at: .venv
⠋ my_personal_project==1.3.1
```

`.python-version`
```
3.13.2
```

`pyproject.toml`
```toml
[project]
name = "my_personal_project"
version = "1.3.1"
authors = [
    { name = "VERBOSE", email = "a@b.c" },
]
requires-python = ">=3.10"
dependencies = [
    "beautifulsoup4==4.12.2",
    "openpyxl==3.1.5",
    # ... others
]

[dependencies-groups]
dev = [
    "pytest==6.2.5",
    "ruff==0.11.0",
]
```

#### Some context
I'm dropping `requirements.txt` and `requirements-dev.txt` in favor of `uv.lock`. It means that I have no reqs files anymore and the `pyproject.toml` was created manually (before installing `uv`).


### Platform

Windows 10

### Version

0.6.7

---

_Label `question` added by @VERBOSE-01 on 2025-03-18 09:07_

---

_Comment by @charliermarsh on 2025-03-18 13:49_

It looks like we're _resolving_ your project (which we'd need to do for dependency resolution) but we may not be installing it. Can you please create a complete reproduction -- like a GitHub repository we can clone and a set of commands we can run?

---

_Label `question` removed by @charliermarsh on 2025-03-18 13:49_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-18 13:49_

---

_Comment by @VERBOSE-01 on 2025-03-18 14:43_

> It looks like we're _resolving_ your project (which we'd need to do for dependency resolution) but we may not be installing it. Can you please create a complete reproduction -- like a GitHub repository we can clone and a set of commands we can run?

Many thanks for taking the time. I'm afraid I won't be able to make a MRE because it isn't an issue for me anymore. Maybe it's completely random but I just did the following steps (see below) and uv installed all dependencies and created a `uv.lock` successfully: 

1. run `$env:HTTPS_PROXY = "http://..."` since I'm behind a proxy (as suggested [here](https://github.com/astral-sh/uv/issues/1474#issuecomment-2519671631))
2. deleted the old `.venv` (created earlier by `uv sync --no-install-project`)
3. replaced `[dependencies-groups]` with `dependency-groups` in the `pyproject.toml`
4. created a new `.venv` and this time with `uv venv --seed` (as suggested [here](https://github.com/astral-sh/uv/issues/1698#issuecomment-1952746172) because my pip install was ignoring `.venv`)
5. re run `uv sync` (without any flag this time)

#### Vscode terminal (powershell)
```bash
PS C:\Users\verbose\Desktop\my_personal_project> $env:HTTPS_PROXY = "http://..."

PS C:\Users\verbose\Desktop\my_personal_project> uv venv --seed
Using CPython 3.13.2 interpreter at: C:\Users\verbose\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment with seed packages at: .venv
 + pip==25.0.1
Activate with: .venv\Scripts\activate

PS C:\Users\verbose\Desktop\my_personal_project> uv sync
Using CPython 3.13.2 interpreter at: C:\Users\verbose\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment at: .venv
Resolved x packages in xms
```

Sorry again and thanks for this tool. A game changer, honestly ;)

---

_Comment by @sanmai-NL on 2025-03-20 10:26_

@VERBOSE-01 Can this be closed then?

---

_Closed by @charliermarsh on 2025-03-24 15:18_

---
