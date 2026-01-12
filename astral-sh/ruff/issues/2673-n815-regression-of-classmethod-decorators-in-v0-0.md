```yaml
number: 2673
title: "`N815`: Regression of `classmethod-decorators` in v0.0.244"
type: issue
state: closed
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2023-02-08T23:56:50Z
updated_at: 2023-02-09T02:11:38Z
url: https://github.com/astral-sh/ruff/issues/2673
synced_at: 2026-01-12T15:54:43Z
```

# `N815`: Regression of `classmethod-decorators` in v0.0.244

---

_@ngnpope_

For this code:

```python
import pydantic

class Thing(pydantic.BaseModel):
    @pydantic.root_validator(pre=True)
    def _validate_something(cls, values):
        return values
```

And this configuration:

```toml
[pep8-naming]
classmethod-decorators = ["classmethod", "pydantic.root_validator"]
```

I get this output:

```console
❯ pipx inject ruff ruff==0.0.243
  injected package ruff into venv ruff
done! ✨ 🌟 ✨

❯ ruff --version
ruff 0.0.243

❯ ruff --config bug.toml --select N805 bug.py 

❯ pipx inject ruff ruff==0.0.244
  injected package ruff into venv ruff
done! ✨ 🌟 ✨

❯ ruff --version
ruff 0.0.244

❯ ruff --config bug.toml --select N805 bug.py 
bug.py:5:29: N805 First argument of a method should be named `self`
Found 1 error.
```

---

_Comment by @charliermarsh on 2023-02-09 00:02_

Sorry about that, will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-09 00:02_

---

_Label `bug` added by @charliermarsh on 2023-02-09 00:02_

---

_Comment by @charliermarsh on 2023-02-09 00:04_

I actually tested that `staticmethod()` doesn't work, but forgot users could provide their own decorators here 😬 

---

_Closed by @charliermarsh on 2023-02-09 02:11_

---
