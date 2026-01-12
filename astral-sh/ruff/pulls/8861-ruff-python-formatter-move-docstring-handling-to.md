```yaml
number: 8861
title: "ruff_python_formatter: move docstring handling to a submodule"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fmt/move-docstring
created_at: 2023-11-27T17:35:41Z
updated_at: 2023-11-27T18:32:27Z
url: https://github.com/astral-sh/ruff/pull/8861
synced_at: 2026-01-12T15:55:27Z
```

# ruff_python_formatter: move docstring handling to a submodule

---

_@BurntSushi_

This turns `string` into a parent module with a `docstring` sub-module. I arranged things this way because there are parts of the `string` module that the `docstring` module wants to know about (such as a `NormalizedString`). The alternative I think would be to make `docstring` a sibling module and expose more of `string`'s internals.

I think I overall like this change because it gives docstring handling a bit more room to breath. It has grown quite a bit with the addition of code snippet formatting.

[This was suggested by @charliermarsh.](https://github.com/astral-sh/ruff/pull/8811#discussion_r1401169531)

---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-27 17:35_

---

_Comment by @github-actions[bot] on 2023-11-27 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2023-11-27 18:04_

Great, thanks for following up!

---

_Label `internal` added by @charliermarsh on 2023-11-27 18:04_

---

_Merged by @BurntSushi on 2023-11-27 18:32_

---

_Closed by @BurntSushi on 2023-11-27 18:32_

---

_Branch deleted on 2023-11-27 18:32_

---
