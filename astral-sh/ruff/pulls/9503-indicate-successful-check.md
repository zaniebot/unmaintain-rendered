```yaml
number: 9503
title: Indicate successful check
type: pull_request
state: closed
author: hughhan1
labels: []
assignees: []
base: main
head: indicate-successful-check
created_at: 2024-01-13T07:14:05Z
updated_at: 2024-03-11T18:57:33Z
url: https://github.com/astral-sh/ruff/pull/9503
synced_at: 2026-01-10T22:47:01Z
```

# Indicate successful check

---

_Pull request opened by @hughhan1 on 2024-01-13 07:14_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This implements outputting a message to `stdout` when a check is run and errors do not occur and fixes are not made. This is done only for when watch mode is _**not**_ enabled; running via watch mode will continue to not output anything when errors do not occur and fixes are not made.

See https://github.com/astral-sh/ruff/issues/8553 for more information.

## Test Plan

Snapshot tests.


---

_Review comment by @hughhan1 on `crates/ruff_cli/src/lib.rs`:405 on 2024-01-13 07:15_

Flagging here that I'm returning `1` here for the number of affected files if we're reading from `stdin`.

Please let me know if this is okay!

---

_Review comment by @hughhan1 on `crates/ruff_cli/tests/integration_test.rs`:1111 on 2024-01-13 07:16_

Flagging here that we're displaying the `Found 0 errors in 1 file`, even when we run into a permission-denied error. Perhaps the output here is undesirable?

Open to feedback and suggestions on what we think the ideal product behavior should look like!

---

_Review comment by @hughhan1 on `crates/ruff_cli/src/lib.rs`:436 on 2024-01-13 07:17_

Flagging the logic here on how I'm deciding whether or not to display the "success" output. Please let me know if we think the heuristic should be different!

---

_@hughhan1 reviewed on 2024-01-13 07:17_

---

_Marked ready for review by @hughhan1 on 2024-01-13 07:18_

---

_Comment by @hughhan1 on 2024-01-13 07:22_

This is my first time attempting to contribute to the Ruff codebase, so please let me know if my logic is misdirected, e.g. at the incorrect abstraction levels. 

I'm really trying to familiarize myself with the codebase so any direction or guidance is greatly appreciated!

---

_Comment by @github-actions[bot] on 2024-01-13 07:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @hughhan1 on 2024-01-13 16:48_

Ahh, I just realized that https://github.com/astral-sh/ruff/pull/8631 already exists, and is probably at the more correct level of abstraction to implement this in!

---

_Comment by @zanieb on 2024-03-11 18:57_

https://github.com/astral-sh/ruff/pull/8631 does seem more appropriate, we'll try to get that one merged.

Thank you for contributing though! I'm interested in adding those file counts 👀 

---

_Closed by @zanieb on 2024-03-11 18:57_

---
