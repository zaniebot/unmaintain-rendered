---
number: 10865
title: "ruff doesn't work with poetry"
type: issue
state: closed
author: sdan
labels:
  - needs-info
assignees: []
created_at: 2024-04-10T21:20:19Z
updated_at: 2024-04-11T01:04:28Z
url: https://github.com/astral-sh/ruff/issues/10865
synced_at: 2026-01-10T01:22:50Z
---

# ruff doesn't work with poetry

---

_Issue opened by @sdan on 2024-04-10 21:20_

on vscode macOS sonoma

```
Ruff: Lint failed (error: TOML parse error at line 312, column 12 | 312 | [tool.ruff.lint] | ^^^^ unknown field `lint`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `extend-fixable`, `extend-unfixable`, `external`, `fix`, `fix-only`, `fixable`, `format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `tab-size`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-arguments`, `iso...
```

```
Ruff: Fix failed (error: TOML parse error at line 75, column 12 | 75 | [tool.ruff.lint.per-file-ignores] | ^^^^ unknown field `lint`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `extend-fixable`, `extend-unfixable`, `external`, `fix`, `fix-only`, `fixable`, `format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `tab-size`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-argu...
```

---

_Renamed from "ruff doesn't work with puppetry" to "ruff doesn't work with poetry" by @sdan on 2024-04-10 21:20_

---

_Comment by @charliermarsh on 2024-04-10 21:25_

Can you share a bit more? How are you running Ruff? What version are you using? It looks like the Ruff configuration can't be parsed. Can you share the full configuration? My best guess from the above is that you're using a very old Ruff version with a newer configuration schema.

---

_Label `needs-info` added by @charliermarsh on 2024-04-10 21:25_

---

_Comment by @sdan on 2024-04-11 01:04_

I had to switch to an old ruff version 0.1.15 to get it to work, thanks!

---

_Closed by @sdan on 2024-04-11 01:04_

---
