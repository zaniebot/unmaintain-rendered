```yaml
number: 746
title: Add support for import alias tracking
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/aliases
created_at: 2022-11-15T01:21:39Z
updated_at: 2022-11-15T02:29:31Z
url: https://github.com/astral-sh/ruff/pull/746
synced_at: 2026-01-12T15:55:05Z
```

# Add support for import alias tracking

---

_@charliermarsh_

This PR enables Ruff to follow aliased imports, like:

```py
import typing as t

t.List[...]
```

...or...:

```py
from typing import List as IList

IList[...]
```

Resolves #720.

Resolves #733.

---

_Merged by @charliermarsh on 2022-11-15 02:29_

---

_Closed by @charliermarsh on 2022-11-15 02:29_

---

_Branch deleted on 2022-11-15 02:29_

---
