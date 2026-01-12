```yaml
number: 1047
title: Add an option to force one-member-per-line for aliased import-froms
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/one-per-line
created_at: 2022-12-05T01:48:45Z
updated_at: 2022-12-05T02:22:03Z
url: https://github.com/astral-sh/ruff/pull/1047
synced_at: 2026-01-12T15:55:05Z
```

# Add an option to force one-member-per-line for aliased import-froms

---

_@charliermarsh_

This PR adds a `force-wrap-aliases` setting, which lets you preserve one-per-line imports when using import aliases, like so:

```py
from .utils import (
    test_directory as test_directory,
    test_id as test_id,
)
```

Specifically, we enforce multi-line formatting when this setting is enabled, the import-from has more than one member, and at least one member is aliased.

Resolves: #1046.


---

_Merged by @charliermarsh on 2022-12-05 02:22_

---

_Closed by @charliermarsh on 2022-12-05 02:22_

---

_Branch deleted on 2022-12-05 02:22_

---
