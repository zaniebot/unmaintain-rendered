```yaml
number: 8480
title: Added tabs for configuration files in the documentation
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: config-tabs
created_at: 2023-11-03T21:08:18Z
updated_at: 2023-11-05T18:48:17Z
url: https://github.com/astral-sh/ruff/pull/8480
synced_at: 2026-01-12T15:55:26Z
```

# Added tabs for configuration files in the documentation

---

_@trag1c_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #8384.

## Test Plan

Checked whether it renders properly on the `mkdocs serve` preview.


---

_Comment by @trag1c on 2023-11-03 21:22_

It seems like the syntax required by the tabs plugin goes against `mdformat`'s requirements. Should the hook be removed from pre-commit?

Edit: I excluded the docs folder from mdformat and markdownlint hooks

---

_Comment by @github-actions[bot] on 2023-11-03 21:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2023-11-05 17:01_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_options.rs`:6 on 2023-11-05 17:01_

Ohh I didn't realize when you asked this that it was specifically for Ruff :joy:

---

_@trag1c reviewed on 2023-11-05 17:02_

---

_Review comment by @trag1c on `crates/ruff_dev/src/generate_options.rs`:6 on 2023-11-05 17:02_

:D

---

_@charliermarsh approved on 2023-11-05 17:03_

This is excellent, thank you so much!

---

_Label `documentation` added by @charliermarsh on 2023-11-05 17:03_

---

_Comment by @charliermarsh on 2023-11-05 17:04_

(Fixing Clippy, which I broke.)

---

_Merged by @charliermarsh on 2023-11-05 17:10_

---

_Closed by @charliermarsh on 2023-11-05 17:10_

---

_Comment by @doolio on 2023-11-05 18:48_

Great job @trag1c. This makes it so much better for me as a beginner.

---
