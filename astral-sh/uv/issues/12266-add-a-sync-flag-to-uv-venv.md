```yaml
number: 12266
title: "Add a --sync flag to `uv venv`"
type: issue
state: closed
author: AtomToast
labels:
  - enhancement
assignees: []
created_at: 2025-03-18T09:11:25Z
updated_at: 2025-03-18T13:50:58Z
url: https://github.com/astral-sh/uv/issues/12266
synced_at: 2026-01-12T16:00:59Z
```

# Add a --sync flag to `uv venv`

---

_@AtomToast_

### Summary

Add some sort of flag to the `uv venv` command to automatically run `uv sync` inside of the new virtual environment. Alternatively some other method of automatically populating the virtual environments with the contents of for example a pyproject.toml

### Example

Instead of having to potentially run 3 commands:

```bash
uv venv
source .venv/bin/activate
uv sync
```

this could be condensed down to just one command

```bash
uv venv --sync
```

---

_Label `enhancement` added by @AtomToast on 2025-03-18 09:11_

---

_Comment by @FishAlchemist on 2025-03-18 10:35_

``uv sync`` already includes creating a virtual environment and installing dependencies.
* https://docs.astral.sh/uv/reference/cli/#uv-sync
* https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path

Also, using uv doesn't actually require entering a virtual environment to install dependencies.
* https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments
* https://docs.astral.sh/uv/concepts/python-versions/#finding-a-python-executable

---

_Comment by @nbaju1 on 2025-03-18 12:30_

Would be nice to have if one wants a custom name for the venv.

---

_Comment by @charliermarsh on 2025-03-18 13:50_

I appreciate the issue but we likely won't implement this. I think this is better served by existing commands, and extending `uv venv` like this would create another way to do the same things.

---

_Closed by @charliermarsh on 2025-03-18 13:50_

---
