```yaml
number: 14923
title: "[red-knot] Make `attributes.md` test future-proof"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-attributed_md-test
created_at: 2024-12-11T19:40:20Z
updated_at: 2024-12-11T19:46:28Z
url: https://github.com/astral-sh/ruff/pull/14923
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Make `attributes.md` test future-proof

---

_@sharkdp_

## Summary

Using `typing.LiteralString` breaks as soon as we understand `sys.version_info` branches, as it's only available in 3.11 and later. 

## Test Plan

Made sure it didn't fail on my #14759 branch anymore.


---

_Label `red-knot` added by @sharkdp on 2024-12-11 19:40_

---

_Review requested from @carljm by @sharkdp on 2024-12-11 19:40_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-11 19:40_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-11 19:40_

---

_Comment by @AlexWaygood on 2024-12-11 19:41_

`typing.Literal` was added in 3.8

---

_Comment by @AlexWaygood on 2024-12-11 19:42_

Oh, you mean `LiteralString`! Yes, that's correct. Sorry for breaking your branch 🙃

---

_@AlexWaygood approved on 2024-12-11 19:42_

---

_Comment by @sharkdp on 2024-12-11 19:45_

> Oh, you mean `LiteralString`

Yes, thanks!

> Sorry for breaking your branch

No worries — I just wanted to pull it out from #14759 which seems big enough already. And I wasn't sure if you'd be okay with the change here.

---

_Merged by @sharkdp on 2024-12-11 19:46_

---

_Closed by @sharkdp on 2024-12-11 19:46_

---

_Comment by @github-actions[bot] on 2024-12-11 19:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-12-11 19:46_

---
