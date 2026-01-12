```yaml
number: 14694
title: "[`refurb`] Guard `hashlib` imports and mark `hashlib-digest-hex` fix as safe (`FURB181`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
assignees: []
merged: true
base: main
head: hashlib-digest-hex-safe
created_at: 2024-11-30T19:31:52Z
updated_at: 2024-12-02T01:27:45Z
url: https://github.com/astral-sh/ruff/pull/14694
synced_at: 2026-01-12T15:55:48Z
```

# [`refurb`] Guard `hashlib` imports and mark `hashlib-digest-hex` fix as safe (`FURB181`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Check if `hashlib` and `crypt` imports have been seen for `FURB181` and `S324`
- Mark the fix for `FURB181` as safe: I think it was accidentally marked as unsafe in the first place. The rule does not support user-defined classes as the "fix safety" section suggests.
- Removed `hashlib._Hash`, as it's not part of the `hashlib` module.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Updated the test snapshots


---

_Comment by @github-actions[bot] on 2024-11-30 19:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-12-02 01:24_

This makes sense, thanks.

---

_Merged by @charliermarsh on 2024-12-02 01:24_

---

_Closed by @charliermarsh on 2024-12-02 01:24_

---

_Label `fixes` added by @charliermarsh on 2024-12-02 01:24_

---

_Branch deleted on 2024-12-02 01:27_

---
