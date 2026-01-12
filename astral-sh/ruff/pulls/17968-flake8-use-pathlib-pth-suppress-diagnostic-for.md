```yaml
number: 17968
title: "[`flake8-use-pathlib`] `PTH*` suppress diagnostic for all `os.*` functions that have the `dir_fd` parameter"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-PTH116
created_at: 2025-05-08T21:32:25Z
updated_at: 2025-05-12T20:23:20Z
url: https://github.com/astral-sh/ruff/pull/17968
synced_at: 2026-01-12T15:56:09Z
```

# [`flake8-use-pathlib`] `PTH*` suppress diagnostic for all `os.*` functions that have the `dir_fd` parameter

---

_@LaBatata101_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #17776.

This PR also handles all other `PTH*` rules that don't support file descriptors.

## Test Plan

<!-- How was it tested? -->

Update existing tests.


---

_Marked ready for review by @LaBatata101 on 2025-05-08 21:32_

---

_Comment by @github-actions[bot] on 2025-05-08 21:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-05-09 06:39_

---

_Assigned to @ntBre by @ntBre on 2025-05-09 12:35_

---

_Label `bug` added by @ntBre on 2025-05-09 19:23_

---

_Label `rule` added by @ntBre on 2025-05-09 19:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:39 on 2025-05-09 19:27_

I think we should use `find_argument_value` for the first argument too, as I think it's okay to pass as a kwarg:

```pycon
>>> import os
>>> os.chmod(path="try", mode=0o755)
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:62 on 2025-05-09 19:29_

nit: I think we could add a little helper for this taking `call.arguments` and the arguments to `find_argument_value`.

---

_@ntBre reviewed on 2025-05-09 19:34_

Thanks! Just a couple of minor suggestions. I didn't cross-check these  with the docs, but they all look good to me, and I appreciated the comments with the signatures.

---

_@LaBatata101 reviewed on 2025-05-09 19:39_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:39 on 2025-05-09 19:39_

Yeah, you are right.

---

_@LaBatata101 reviewed on 2025-05-09 20:04_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:39 on 2025-05-09 20:04_

I've updated all the rules where this could happen.

---

_Review requested from @ntBre by @LaBatata101 on 2025-05-09 20:05_

---

_@ntBre approved on 2025-05-12 20:11_

Thanks!

---

_Merged by @ntBre on 2025-05-12 20:11_

---

_Closed by @ntBre on 2025-05-12 20:11_

---

_Branch deleted on 2025-05-12 20:23_

---
