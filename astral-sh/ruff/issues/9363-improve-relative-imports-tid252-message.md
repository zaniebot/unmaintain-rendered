```yaml
number: 9363
title: "Improve `relative-imports` (`TID252`) message"
type: issue
state: closed
author: tjkuson
labels:
  - documentation
assignees: []
created_at: 2024-01-02T18:31:25Z
updated_at: 2024-01-02T20:11:25Z
url: https://github.com/astral-sh/ruff/issues/9363
synced_at: 2026-01-12T15:54:49Z
```

# Improve `relative-imports` (`TID252`) message

---

_@tjkuson_

Running `ruff check --select TID252 /tmp/example.py` using version `0.1.9` on

```python
from .. import foo
```

outputs

```
/tmp/example.py:1:1: TID252 Relative imports from parent modules are banned
```

I don't think this is a helpful message as it implies such imports don't work and doesn't suggest an improvement. It also doesn't match the style of other Ruff rule messages.

How about `Prefer absolute imports to relative imports from parent modules`?


---

_Comment by @charliermarsh on 2024-01-02 18:33_

Makes sense to me!

---

_Label `documentation` added by @charliermarsh on 2024-01-02 18:33_

---

_Closed by @charliermarsh on 2024-01-02 20:11_

---
