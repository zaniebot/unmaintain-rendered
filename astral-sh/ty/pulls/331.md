```yaml
number: 331
title: Split the documentation out of the top-level README
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-split
created_at: 2025-05-12T18:15:42Z
updated_at: 2025-05-12T18:45:04Z
url: https://github.com/astral-sh/ty/pull/331
synced_at: 2026-01-10T02:34:10Z
```

# Split the documentation out of the top-level README

---

_Pull request opened by @zanieb on 2025-05-12 18:15_

Moves most of the documentation into a dedicated `docs/README` to make it easier to reach important content in the top-level readme like "getting involved".

There are some minor changes to the heading levels here and link changes, but otherwise the content is the same. Don't go deep on the getting started section, that's changing in #329 

---

_Label `documentation` added by @zanieb on 2025-05-12 18:15_

---

_Marked ready for review by @zanieb on 2025-05-12 18:23_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:44 on 2025-05-12 18:26_

should we exclude these from all pre-commit hooks if they're generated files? If so, I'd add them to the global `exclude` regex at the top of this file rather than hook-specific `exclude` regexes

---

_@AlexWaygood reviewed on 2025-05-12 18:26_

---

_@zanieb reviewed on 2025-05-12 18:37_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:44 on 2025-05-12 18:37_

I just copy pasted from Ruff's config

---

_@zanieb reviewed on 2025-05-12 18:38_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:44 on 2025-05-12 18:38_

Oh, that's how it was passing before... these were in the top-level one

---

_@zanieb reviewed on 2025-05-12 18:39_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:44 on 2025-05-12 18:39_

Thanks!

---

_@MichaReiser approved on 2025-05-12 18:44_

---

_Merged by @zanieb on 2025-05-12 18:45_

---

_Closed by @zanieb on 2025-05-12 18:45_

---

_Branch deleted on 2025-05-12 18:45_

---
