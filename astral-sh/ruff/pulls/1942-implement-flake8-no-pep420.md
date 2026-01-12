```yaml
number: 1942
title: "Implement `flake8-no-pep420`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: flake8-no-pep420
created_at: 2023-01-18T02:18:33Z
updated_at: 2023-01-18T03:19:52Z
url: https://github.com/astral-sh/ruff/pull/1942
synced_at: 2026-01-12T05:25:13Z
```

# Implement `flake8-no-pep420`

---

_Pull request opened by @edgarrmondragon on 2023-01-18 02:18_

Closes https://github.com/charliermarsh/ruff/issues/1844


---

_@charliermarsh reviewed on 2023-01-18 02:20_

---

_Review comment by @charliermarsh on `src/linter.rs`:114 on 2023-01-18 02:20_

I think this can be moved to the top-level (outside of the `if use_ast || use_imports || use_doc_lines || use_filesystem {`), since it doesn't rely on the AST. (That's the purpose of this block: to only parse the AST if we need to, then share it between the checkers.)

---

_Merged by @charliermarsh on 2023-01-18 03:10_

---

_Closed by @charliermarsh on 2023-01-18 03:10_

---

_Branch deleted on 2023-01-18 03:19_

---
