```yaml
number: 22572
title: "feat: added isort options for completeness"
type: pull_request
state: open
author: tonycoco
labels: []
assignees: []
base: main
head: isort-options
created_at: 2026-01-14T15:34:41Z
updated_at: 2026-01-14T15:34:41Z
url: https://github.com/astral-sh/ruff/pull/22572
synced_at: 2026-01-14T15:39:42Z
```

# feat: added isort options for completeness

---

_@tonycoco_

WIP to add completeness for isort options like:

```toml
[tool.ruff.lint.isort]
lexicographical = true
line-length = 1000
group-by-package = true
force-single-line = true
force-sort-within-sections = true
single-line-exclusions = [
  "collections.abc",
  "six.moves",
  "typing",
  "typing-extensions"
]
order-by-type = false
```


---
