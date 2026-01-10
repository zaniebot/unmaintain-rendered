```yaml
number: 5434
title: uv sync in empty workspace succeeds and fails
type: issue
state: closed
author: bluss
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-25T06:37:46Z
updated_at: 2024-07-25T18:01:42Z
url: https://github.com/astral-sh/uv/issues/5434
synced_at: 2026-01-10T04:53:49Z
```

# uv sync in empty workspace succeeds and fails

---

_Issue opened by @bluss on 2024-07-25 06:37_

Papercut type thing, not important.

Steps to reproduce

```
mkdir project; cd project
uv init --virtual
uv sync
uv sync
```

Actual output

```
Failed to parse lockfile; ignoring locked requirements: TOML parse error at line 1, column 1
  |
1 | version = 1
  | ^^^^^^^^^^^
missing field `distribution`
```

First sync runs successfully, creates the following lockfile.
Second sync does not accept reading it.

```toml
version = 1
requires-python = ">=3.12"
```

uv 0.2.29

---

_Label `bug` added by @konstin on 2024-07-25 09:17_

---

_Label `preview` added by @konstin on 2024-07-25 09:17_

---

_Comment by @charliermarsh on 2024-07-25 17:27_

Oops, good call.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-25 17:28_

---

_Closed by @charliermarsh on 2024-07-25 18:01_

---

_Closed by @charliermarsh on 2024-07-25 18:01_

---
