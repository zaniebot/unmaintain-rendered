```yaml
number: 9963
title: "unused_imports/F401: Explain when imports are preserved"
type: pull_request
state: merged
author: Hnasar
labels:
  - documentation
assignees: []
merged: true
base: main
head: unused-import-docs
created_at: 2024-02-12T20:58:48Z
updated_at: 2024-02-13T00:07:20Z
url: https://github.com/astral-sh/ruff/pull/9963
synced_at: 2026-01-12T15:55:30Z
```

# unused_imports/F401: Explain when imports are preserved

---

_@Hnasar_

The docs previously mentioned an irrelevant config option, but were missing a link to the relevant `ignore-init-module-imports` config option which _is_ actually used.

Additionally, this commit adds a link to the documentation to explain the conventions around a module interface which includes using a redundant import alias to preserve an unused import.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

(noticed this while filing  #9962)


---

_Comment by @github-actions[bot] on 2024-02-12 21:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @zanieb on 2024-02-12 21:24_

---

_@charliermarsh approved on 2024-02-13 00:07_

Thanks!

---

_Merged by @charliermarsh on 2024-02-13 00:07_

---

_Closed by @charliermarsh on 2024-02-13 00:07_

---
