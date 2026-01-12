```yaml
number: 3119
title: "Feature: Support isort force_alphabetical_sort"
type: issue
state: open
author: jensens
labels:
  - isort
assignees: []
created_at: 2023-02-22T11:17:12Z
updated_at: 2025-11-23T09:33:33Z
url: https://github.com/astral-sh/ruff/issues/3119
synced_at: 2026-01-12T15:54:43Z
```

# Feature: Support isort force_alphabetical_sort

---

_@jensens_

The isort module provides a way to sort alphabetically using `force_alphabetical_sort = true`, which is very handy in large projects. In Plone we use use this option - it's part of the style-guide. 

Ideally, the ruff setting would be something like this:

[tool.ruff.isort]
force-alphabetical-sort

This follows the [isort option setting](https://pycqa.github.io/isort/docs/configuration/options.html#force-alphabetical-sort) but with underscores replaced by dashes.

---

_Label `isort` added by @charliermarsh on 2023-02-22 14:26_

---

_Comment by @hofrob on 2023-02-23 23:07_

A similarly neat one is `force-alphabetical-sort-within-sections`: https://pycqa.github.io/isort/docs/configuration/options.html#force-alphabetical-sort-within-sections

---

_Comment by @jensens on 2025-11-23 09:33_

Are there any intentions to add this feature? It is the only reason I still use isort in many projects instead of ruff only. 

---
