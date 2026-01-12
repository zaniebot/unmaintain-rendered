```yaml
number: 673
title: Ignore default major and minor labels during version determination
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/rooster-maj-min
created_at: 2025-06-17T20:09:22Z
updated_at: 2025-06-17T20:19:10Z
url: https://github.com/astral-sh/ty/pull/673
synced_at: 2026-01-12T15:54:27Z
```

# Ignore default major and minor labels during version determination

---

_@zanieb_

Otherwise, a `breaking` label will move us to 1.0.0

---

_Label `internal` added by @zanieb on 2025-06-17 20:09_

---

_Marked ready for review by @zanieb on 2025-06-17 20:12_

---

_@zanieb reviewed on 2025-06-17 20:14_

---

_Review comment by @zanieb on `pyproject.toml`:110 on 2025-06-17 20:14_

See defaults at https://github.com/zanieb/rooster/blob/da7f2f6e57a98f05ed2ad611dccc30c73aa1a613/src/rooster/_config.py#L29-L31

`patch-labels` is actually available there too, but unused 🙃 

---

_@AlexWaygood approved on 2025-06-17 20:18_

---

_Merged by @zanieb on 2025-06-17 20:19_

---

_Closed by @zanieb on 2025-06-17 20:19_

---

_Branch deleted on 2025-06-17 20:19_

---
