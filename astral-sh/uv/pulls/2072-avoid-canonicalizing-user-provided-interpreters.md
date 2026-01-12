```yaml
number: 2072
title: Avoid canonicalizing user-provided interpreters
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pyenv
created_at: 2024-02-29T02:17:56Z
updated_at: 2024-02-29T02:32:54Z
url: https://github.com/astral-sh/uv/pull/2072
synced_at: 2026-01-12T16:04:51Z
```

# Avoid canonicalizing user-provided interpreters

---

_@charliermarsh_

## Summary

We shouldn't be resolving symlinks on the provided interpreter; otherwise we break `pyenv`, since running `cargo run pip install mypy --python .venv/bin/python` will immediately resolve to (e.g.) `/Users/crmarsh/.pyenv/versions/3.10.2/bin/python3.10`, and pyenv relies on the path to do its lookups.

Instead, the canonicalizing happens when we query the interpreter metadata.

Closes https://github.com/astral-sh/uv/issues/2068.

## Test Plan

Ran `cargo run pip install mypy --python .venv/bin/python -v -n` with a virtualenv created using a pyenv Python; verified that Mypy was installed into the virtual environment, rather than into the global environment.


---

_Label `bug` added by @charliermarsh on 2024-02-29 02:17_

---

_Marked ready for review by @charliermarsh on 2024-02-29 02:32_

---

_Merged by @charliermarsh on 2024-02-29 02:32_

---

_Closed by @charliermarsh on 2024-02-29 02:32_

---

_Branch deleted on 2024-02-29 02:32_

---
