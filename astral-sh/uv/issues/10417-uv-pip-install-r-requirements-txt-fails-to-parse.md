---
number: 10417
title: "`uv pip install -r requirements.txt` Fails to Parse Requirements"
type: issue
state: closed
author: merajhashemi
labels: []
assignees: []
created_at: 2025-01-08T23:08:22Z
updated_at: 2025-01-08T23:46:27Z
url: https://github.com/astral-sh/uv/issues/10417
synced_at: 2026-01-10T01:24:53Z
---

# `uv pip install -r requirements.txt` Fails to Parse Requirements

---

_Issue opened by @merajhashemi on 2025-01-08 23:08_

Running `uv pip install -r requirements.txt` fails with a parsing error, while` pip install -r requirements.txt` works without issues.

**`requirements.txt` content:**
```
--editable '.[dev]'
```

**Error Output:**
Running `uv pip install -r requirements.txt` produces the following error:
```
error: Couldn't parse requirement in `requirements.txt` at position 11
  Caused by: Expected package name starting with an alphanumeric character, found `'`
'.[dev]'
^
```
However, `pip install -r requirements.txt` successfully installs the requirements.

uv version: 0.5.16


---

_Comment by @charliermarsh on 2025-01-08 23:17_

We don't support stringified dependencies like that. You can just remove the quotes -- they aren't needed in a `requirements.txt` file.

---

_Comment by @charliermarsh on 2025-01-08 23:18_

This is tracked in https://github.com/astral-sh/uv/issues/3621.

---

_Closed by @charliermarsh on 2025-01-08 23:18_

---

_Comment by @merajhashemi on 2025-01-08 23:46_

@charliermarsh 
> We don't support stringified dependencies like that. You can just remove the quotes -- they aren't needed in a `requirements.txt` file.

Without the quotes `uv pip` works; however, pip fails with the following error:
```
ERROR: .[dev, is not a valid editable requirement. It should either be a path to a local project or a VCS URL (beginning with
 bzr+http, bzr+https, bzr+ssh, bzr+sftp, bzr+ftp, bzr+lp, bzr+file, git+http, git+https, git+ssh, git+git, git+file, hg+file, hg+http,
 hg+https, hg+ssh, hg+static-http, svn+ssh, svn+http, svn+https, svn+svn, svn+file).
```
I reckon this difference in behavior should be documented in the pip compatibility section.


---

_Referenced in [astral-sh/uv#10941](../../astral-sh/uv/issues/10941.md) on 2025-01-24 18:10_

---
