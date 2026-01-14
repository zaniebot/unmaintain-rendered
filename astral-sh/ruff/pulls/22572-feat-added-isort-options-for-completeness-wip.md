```yaml
number: 22572
title: "feat: added isort options for completeness (WIP)"
type: pull_request
state: open
author: tonycoco
labels:
  - isort
  - configuration
assignees: []
base: main
head: isort-options
created_at: 2026-01-14T15:34:41Z
updated_at: 2026-01-14T15:48:06Z
url: https://github.com/astral-sh/ruff/pull/22572
synced_at: 2026-01-14T16:39:05Z
```

# feat: added isort options for completeness (WIP)

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

_Renamed from "feat: added isort options for completeness" to "feat: added isort options for completeness (WIP)" by @tonycoco on 2026-01-14 15:35_

---

_Comment by @ntBre on 2026-01-14 15:47_

Hi, thanks for working on this! I know it's still a work in progress, but I wanted to check if there had been some previous discussion of this in an issue or elsewhere? My understanding is that we mostly intentionally don't implement all of the same configuration options as isort, so I'm not sure we will want to add several new options, especially all at once.

---

_Label `isort` added by @ntBre on 2026-01-14 15:48_

---

_Label `configuration` added by @ntBre on 2026-01-14 15:48_

---
