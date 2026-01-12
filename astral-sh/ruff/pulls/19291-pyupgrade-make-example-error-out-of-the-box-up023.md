```yaml
number: 19291
title: "[`pyupgrade`] Make example error out-of-the-box (`UP023`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-4
created_at: 2025-07-11T19:33:49Z
updated_at: 2025-07-11T21:10:19Z
url: https://github.com/astral-sh/ruff/pull/19291
synced_at: 2026-01-12T15:56:36Z
```

# [`pyupgrade`] Make example error out-of-the-box (`UP023`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [deprecated-c-element-tree (UP023)](https://docs.astral.sh/ruff/rules/deprecated-c-element-tree/#deprecated-c-element-tree-up023)'s example error out-of-the-box. I have no clue why the `import xml.etree.cElementTree` and `from xml.etree import cElementTree` cases are specifically carved out if they do not have an `as ...`, but the tests explicitly call this out, and that's how it is in `pyupgrade`'s source as well.

https://github.com/astral-sh/ruff/blob/b5c5f710fc12b5c512a2e5351684b8ffdf33761f/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP023.py#L23-L31

[Old example](https://play.ruff.rs/632b8ce1-393d-45e5-9504-5444ae71a0d8)
```py
from xml.etree import cElementTree
```

[New example](https://play.ruff.rs/fef4d378-8c54-41b2-8778-2d02bcbbd7d3)
```py
from xml.etree import cElementTree as ET
```

The "Use instead" section was also updated similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-11 19:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-07-11 21:07_

Thanks!

---

_Merged by @dylwil3 on 2025-07-11 21:07_

---

_Closed by @dylwil3 on 2025-07-11 21:07_

---

_Label `documentation` added by @dylwil3 on 2025-07-11 21:07_

---

_Branch deleted on 2025-07-11 21:10_

---
