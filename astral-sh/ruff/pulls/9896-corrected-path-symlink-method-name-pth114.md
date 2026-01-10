```yaml
number: 9896
title: Corrected Path symlink method name (PTH114)
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-pth114-example
created_at: 2024-02-08T17:38:35Z
updated_at: 2024-02-08T18:09:28Z
url: https://github.com/astral-sh/ruff/pull/9896
synced_at: 2026-01-10T22:57:09Z
```

# Corrected Path symlink method name (PTH114)

---

_Pull request opened by @trag1c on 2024-02-08 17:38_

## Summary
Corrects mentions of `Path.is_link` to `Path.is_symlink` (the former doesn't exist).

## Test Plan
```sh
python scripts/generate_mkdocs.py && mkdocs serve -f mkdocs.public.yml
```


---

_Renamed from "Corrected Path symlink method name" to "Corrected Path symlink method name (PTH114)" by @trag1c on 2024-02-08 17:39_

---

_Comment by @github-actions[bot] on 2024-02-08 17:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-02-08 18:09_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-02-08 18:09_

---

_Merged by @charliermarsh on 2024-02-08 18:09_

---

_Closed by @charliermarsh on 2024-02-08 18:09_

---
