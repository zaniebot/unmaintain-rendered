```yaml
number: 7485
title: "Fix `''' \"\"'''` formatting"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: string-formatting-edge-case
created_at: 2023-09-18T09:14:40Z
updated_at: 2023-09-18T10:28:16Z
url: https://github.com/astral-sh/ruff/pull/7485
synced_at: 2026-01-12T02:39:10Z
```

# Fix `''' ""'''` formatting

---

_Pull request opened by @konstin on 2023-09-18 09:14_

## Summary

`''' ""'''` is an edge case that was previously incorrectly formatted as `""" """""`.

Fixes #7460

## Test Plan

Added regression test


---

_Label `formatter` added by @konstin on 2023-09-18 09:14_

---

_@MichaReiser reviewed on 2023-09-18 09:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:556 on 2023-09-18 09:26_

Can you explain the fix? 

I'm asking because I would expect `preferred_quotes` to return single here because it contains two `"`... Changing the quotes to `"` would require two escapes... which is more than when keeping the single quotes.

Edit: I guess it is the same as 562? Maybe expand the comment. 

---

_@MichaReiser reviewed on 2023-09-18 09:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:550 on 2023-09-18 09:27_

Nit: We should break when setting `uses_triple_quotes`

---

_Merged by @konstin on 2023-09-18 10:28_

---

_Closed by @konstin on 2023-09-18 10:28_

---

_Branch deleted on 2023-09-18 10:28_

---
