```yaml
number: 1276
title: Add packaging status badge from repology
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: repology-badge
created_at: 2022-12-18T07:07:46Z
updated_at: 2022-12-18T19:33:28Z
url: https://github.com/astral-sh/ruff/pull/1276
synced_at: 2026-01-12T05:36:31Z
```

# Add packaging status badge from repology

---

_Pull request opened by @andersk on 2022-12-18 07:07_

Preview: https://github.com/andersk/ruff/tree/repology-badge#installation

---

_Comment by @charliermarsh on 2022-12-18 16:49_

Is there any way to hide the AUR entry, since we have the Arch community entry?

---

_Comment by @charliermarsh on 2022-12-18 17:08_

Nevermind, I can handle.

---

_Comment by @charliermarsh on 2022-12-18 19:27_

Looks like it's still being included, but because it's "rolling" rather than "ignored"? Anyway, not a big deal. Thank you!

---

_Merged by @charliermarsh on 2022-12-18 19:27_

---

_Closed by @charliermarsh on 2022-12-18 19:27_

---

_Branch deleted on 2022-12-18 19:30_

---

_Comment by @andersk on 2022-12-18 19:33_

Yeah I didn’t find a way to exclude it without excluding *all* repositories (`?exclude_sources=repository`). There’s a `minversion` parameter but it only changes the color.

---
