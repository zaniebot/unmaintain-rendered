```yaml
number: 922
title: Bugbear extend-immutable-calls with import alias does not work
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2022-11-27T16:15:44Z
updated_at: 2022-11-27T23:39:25Z
url: https://github.com/astral-sh/ruff/issues/922
synced_at: 2026-01-10T12:09:58Z
```

# Bugbear extend-immutable-calls with import alias does not work

---

_Issue opened by @JonathanPlasse on 2022-11-27 16:15_

```python
from fastapi.openapi.models import OAuthFlows as OAuthFlowsModel

def okay_alias(model=OAuthFlowsModel()):
    ...
```

```toml
[tool.ruff]
select = ["B008"]

[tool.ruff.flake8-bugbear]
extend-immutable-calls = ["fastapi.openapi.models.OAuthFlows"]
```

---

_Label `bug` added by @charliermarsh on 2022-11-27 20:37_

---

_Closed by @charliermarsh on 2022-11-27 22:27_

---

_Comment by @JonathanPlasse on 2022-11-27 23:39_

Thanks for the fix.

---
