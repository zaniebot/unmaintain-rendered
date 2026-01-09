---
number: 11053
title: Transfering requirements from pyproject.toml to script drops index
type: issue
state: open
author: BenediktMaag
labels:
  - needs-design
assignees: []
created_at: 2025-01-29T08:52:09Z
updated_at: 2025-01-29T13:54:06Z
url: https://github.com/astral-sh/uv/issues/11053
synced_at: 2026-01-07T13:12:18-06:00
---

# Transfering requirements from pyproject.toml to script drops index

---

_Issue opened by @BenediktMaag on 2025-01-29 08:52_

### Summary

Use case: I have a folder where i write and keep my script relevant to a certain project / dependency setup which references a local index. I want to share a script with a colleague for him to reproduce but I want to share only one necessary file and not a full folder with all the other files he doesnt need.

Setup of the folder:
> uv init <folder-name>
> cd <folder-name>
> uv add <mydependency> --index internal=<path-to-index>

Before sending him the script, I need to copy the dependencies into the script. There are two possible ways i would solve this, both not working as expected:

❯ : uv add --script <script-name>.py -r pyproject.toml
error: Adding requirements from a `pyproject.toml` is not supported in `uv add`

❯ : uv export | uv add --script <script-name>.py -r -
Resolved 3 packages in 194ms
Updated `<script-name>.py`

uv export creates an requirements-txt format which does not include the index nor the linking of the dependency to this index.
When running the script with uv run without it being located in this project folder it will not be able to resolve the dependency as it is missing the index information.

### Platform

Windows 11 x86_64

### Version

uv 0.5.24 (42fae925c 2025-01-23)

### Python version

Python 3.12.5

---

_Label `bug` added by @BenediktMaag on 2025-01-29 08:52_

---

_Referenced in [astral-sh/uv#6637](../../astral-sh/uv/issues/6637.md) on 2025-01-29 08:53_

---

_Label `bug` removed by @charliermarsh on 2025-01-29 13:53_

---

_Label `needs-design` added by @charliermarsh on 2025-01-29 13:53_

---

_Comment by @charliermarsh on 2025-01-29 13:54_

It's intentional that `uv export` drops the index information. `requirements.txt` is much more restrictive, and we can't represent things like explicit or assigned indexes.

---

_Referenced in [astral-sh/uv#12584](../../astral-sh/uv/issues/12584.md) on 2025-04-01 10:23_

---
