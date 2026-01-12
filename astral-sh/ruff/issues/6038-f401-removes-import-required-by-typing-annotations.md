```yaml
number: 6038
title: F401 removes import required by typing annotations
type: issue
state: closed
author: thomas-moulard
labels: []
assignees: []
created_at: 2023-07-24T17:35:32Z
updated_at: 2023-07-24T18:08:38Z
url: https://github.com/astral-sh/ruff/issues/6038
synced_at: 2026-01-12T15:54:45Z
```

# F401 removes import required by typing annotations

---

_@thomas-moulard_

[F401](https://beta.ruff.rs/docs/rules/unused-import/) is a rule that removes unused imports.

The issue is that F401 removes imports that are necessary for typing annotations expressed as comments, see below.

# Repro Case

```py
from typing import Any  # <-- this import is removed by ruff
x = 1  # type: Any
```

## Current Behavior

The import is eliminated erroneously:

```py
x = 1  # type: Any
```

## Desired Behavior

The code is unchanged.

```py
from typing import Any  # <-- this import is removed by ruff
x = 1  # type: Any
```

---

_Comment by @charliermarsh on 2023-07-24 18:08_

I believe this is a duplicate of https://github.com/astral-sh/ruff/issues/1619 (Ruff doesn't parse comment annotations right now).

---

_Closed by @charliermarsh on 2023-07-24 18:08_

---
