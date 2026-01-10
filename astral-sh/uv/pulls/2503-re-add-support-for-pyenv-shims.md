```yaml
number: 2503
title: Re-add support for pyenv shims
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pyenv
created_at: 2024-03-18T02:39:00Z
updated_at: 2024-03-18T02:57:38Z
url: https://github.com/astral-sh/uv/pull/2503
synced_at: 2026-01-10T14:49:08Z
```

# Re-add support for pyenv shims

---

_Pull request opened by @charliermarsh on 2024-03-18 02:39_

## Summary

By running `get_interpreter_info.py` outside of the current working directory, we seem to have broken pyenv shims.

Closes https://github.com/astral-sh/uv/issues/2488.

## Test Plan

Without this change (resolving to the Homebrew Python, even though we start with a shim):

```
DEBUG Starting interpreter discovery for Python @ `python3.11`
DEBUG Probing interpreter info for: /Users/crmarsh/.pyenv/shims/python3.11
DEBUG Found Python 3.11.7 for: /Users/crmarsh/.pyenv/shims/python3.11
Using Python 3.11.7 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
INFO Removing existing directory
Activate with: source .venv/bin/activate
```

With this change:

```
DEBUG Starting interpreter discovery for Python @ `python3.11`
DEBUG Probing interpreter info for: /Users/crmarsh/.pyenv/shims/python3.11
DEBUG Found Python 3.11.1 for: /Users/crmarsh/.pyenv/shims/python3.11
Using Python 3.11.1 interpreter at: /Users/crmarsh/.pyenv/versions/3.11.1/bin/python3.11
Creating virtualenv at: .venv
INFO Removing existing directory
Activate with: source .venv/bin/activate
```


---

_Label `bug` added by @charliermarsh on 2024-03-18 02:39_

---

_Comment by @charliermarsh on 2024-03-18 02:40_

I'll look into adding a test for this: https://github.com/astral-sh/uv/issues/2504

---

_Merged by @charliermarsh on 2024-03-18 02:57_

---

_Closed by @charliermarsh on 2024-03-18 02:57_

---

_Branch deleted on 2024-03-18 02:57_

---
