```yaml
number: 22560
title: "[`pyflakes`] Report more diagnostic for `F811`"
type: pull_request
state: open
author: chirizxc
labels: []
assignees: []
base: main
head: F811
created_at: 2026-01-13T21:14:28Z
updated_at: 2026-01-13T21:24:27Z
url: https://github.com/astral-sh/ruff/pull/22560
synced_at: 2026-01-13T21:36:29Z
```

# [`pyflakes`] Report more diagnostic for `F811`

---

_@chirizxc_

## Summary

See #22554

## Test Plan

`cargo nextest run pyflake`


---

_Comment by @chirizxc on 2026-01-13 21:15_

This behavior makes it closer to the original [`pyflakes`](https://github.com/PyCQA/pyflakes).

---
