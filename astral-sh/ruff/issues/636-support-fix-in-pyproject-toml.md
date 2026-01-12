```yaml
number: 636
title: support fix in pyproject.toml
type: issue
state: closed
author: tekumara
labels: []
assignees: []
created_at: 2022-11-07T09:08:23Z
updated_at: 2022-11-07T14:46:23Z
url: https://github.com/astral-sh/ruff/issues/636
synced_at: 2026-01-12T15:54:40Z
```

# support fix in pyproject.toml

---

_@tekumara_

_pyproject.toml_ with:
```
[tool.ruff]
fix = true
```

fails:
```
$ ruff .
error unknown field `fix`, expected one of `line-length`, `exclude`, `extend-exclude`, `select`, `extend-select`, `ignore`, `extend-ignore`, `per-file-ignores`, `dummy-variable-rgx`, `target-version`, `flake8-annotations`, `flake8-quotes`, `pep8-naming` for key `tool.ruff` at line 60 column 1
```

would be fantastic to allow this :pray:

---

_Label `enhancement` added by @charliermarsh on 2022-11-07 10:21_

---

_Closed by @charliermarsh on 2022-11-07 14:46_

---
