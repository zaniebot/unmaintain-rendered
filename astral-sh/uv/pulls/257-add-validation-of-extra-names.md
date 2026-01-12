```yaml
number: 257
title: Add validation of extra names
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/extra-validate
created_at: 2023-10-31T18:38:34Z
updated_at: 2023-11-01T15:40:44Z
url: https://github.com/astral-sh/uv/pull/257
synced_at: 2026-01-12T16:03:50Z
```

# Add validation of extra names

---

_@zanieb_

Extends #254 

Adds validation of extra names provided by users in `pip-compile` e.g. 

```
error: invalid value 'foo!' for '--extra <EXTRA>': Extra names must start and end with a
letter or digit and may only contain -, _, ., and alphanumeric characters
```

We'll want to add something similar to `PackageName`. I'd be curious to improve the AP, making the unvalidated nature of `::normalize` clear? Perhaps worth pursuing later though as I don't have a better idea.

---

_@konstin approved on 2023-11-01 12:28_

---

_Merged by @zanieb on 2023-11-01 15:40_

---

_Closed by @zanieb on 2023-11-01 15:40_

---

_Branch deleted on 2023-11-01 15:40_

---
