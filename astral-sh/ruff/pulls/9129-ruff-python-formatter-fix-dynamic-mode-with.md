```yaml
number: 9129
title: "ruff_python_formatter: fix 'dynamic' mode with doctests"
type: pull_request
state: merged
author: BurntSushi
labels:
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/fix-i9126
created_at: 2023-12-14T13:11:13Z
updated_at: 2023-12-14T14:53:45Z
url: https://github.com/astral-sh/ruff/pull/9129
synced_at: 2026-01-12T15:55:27Z
```

# ruff_python_formatter: fix 'dynamic' mode with doctests

---

_@BurntSushi_

This fixes a bug where the current indent level was not calculated correctly for doctests. Namely, it didn't account for the extra indent level (in terms of ASCII spaces) used by by the PS1 (`>>> `) and PS2 (`... `) prompts. As a result, lines could extend up to 4 spaces beyond the configured line length limit.

We fix that by passing the `CodeExampleKind` to the `format` routine instead of just the code itself. In this way, `format` can query whether there will be any extra indent added _after_ formatting the code and take that into account for its line length setting.

We add a few regression tests, taken directly from @stinodego's examples.

Fixes #9126


---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-14 13:11_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-14 13:11_

---

_Comment by @github-actions[bot] on 2023-12-14 13:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `docstring` added by @BurntSushi on 2023-12-14 14:44_

---

_Label `formatter` added by @BurntSushi on 2023-12-14 14:44_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-12-14 14:44_

---

_@zanieb approved on 2023-12-14 14:45_

---

_Merged by @BurntSushi on 2023-12-14 14:53_

---

_Closed by @BurntSushi on 2023-12-14 14:53_

---

_Branch deleted on 2023-12-14 14:53_

---
