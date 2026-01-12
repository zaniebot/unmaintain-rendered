```yaml
number: 16718
title: Add Hex Format Option for Consistent Integer Representation
type: pull_request
state: closed
author: dgagn
labels:
  - formatter
assignees: []
base: main
head: ovior/hex-format
created_at: 2025-03-13T23:14:14Z
updated_at: 2025-04-23T15:39:24Z
url: https://github.com/astral-sh/ruff/pull/16718
synced_at: 2026-01-12T15:55:56Z
```

# Add Hex Format Option for Consistent Integer Representation

---

_@dgagn_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR introduces a new formatting option, `hex_format`, which allows users to specify whether hexadecimal numbers should be formatted in uppercase or lowercase. This enhances consistency in integer representation across a codebase already using hex in lowercase.

## Test Plan

- Added tests for hex numbers
- Updated snapshot tests to reflect the new behavior.
- Manually validated formatting results with different hex_format settings.
- The new setting does not break Black compatibility.

---

_Review requested from @MichaReiser by @dgagn on 2025-03-13 23:14_

---

_Comment by @github-actions[bot] on 2025-03-13 23:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @MichaReiser on 2025-03-14 07:06_

---

_Comment by @MichaReiser on 2025-03-14 07:07_

The code changes look good, but I don't think there's enough demand yet that justifies adding a new formatter option, see https://github.com/astral-sh/ruff/issues/10432. We can re-open this PR once there's more demand for it.

---

_Closed by @MichaReiser on 2025-03-14 07:07_

---

_Comment by @RocketMaDev on 2025-04-23 15:39_

Hi @MichaReiser, thanks for the clarification! Just wondering — is there a general threshold for how many users need to express interest before such a formatting option would be considered? Appreciate all the great work on Ruff! 🙌

---
