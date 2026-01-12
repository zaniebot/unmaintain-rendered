```yaml
number: 16849
title: "[`flake8-executables`] Allow `uv run` in shebang line for `shebang-missing-python` (`EXE003`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: shebang-uv
created_at: 2025-03-19T15:26:46Z
updated_at: 2025-03-19T18:12:49Z
url: https://github.com/astral-sh/ruff/pull/16849
synced_at: 2026-01-12T15:55:59Z
```

# [`flake8-executables`] Allow `uv run` in shebang line for `shebang-missing-python` (`EXE003`)

---

_@dylwil3_

Skip the lint for [shebang-missing-python (EXE003)](https://docs.astral.sh/ruff/rules/shebang-missing-python/#shebang-missing-python-exe003) if we find `uv run` on the shebang line.

Closes #13021


---

_Label `rule` added by @dylwil3 on 2025-03-19 15:26_

---

_@charliermarsh approved on 2025-03-19 15:27_

---

_Comment by @github-actions[bot] on 2025-03-19 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-03-19 15:35_

---

_Closed by @dylwil3 on 2025-03-19 15:35_

---

_Comment by @andrei-korshikov on 2025-03-19 16:33_

I believe this line should be changed too:

https://github.com/astral-sh/ruff/blob/98fdc0ebae5b1f8279d2400445acaf3f172a6513/crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs#L39

E.g.
```python
"Shebang should contain `python`, `pytest`, or `uv run`".to_string()
```

---

_Branch deleted on 2025-03-19 18:12_

---
