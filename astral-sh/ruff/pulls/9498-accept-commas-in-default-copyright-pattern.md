```yaml
number: 9498
title: Accept commas in default copyright pattern
type: pull_request
state: merged
author: dopplershift
labels:
  - bug
assignees: []
merged: true
base: main
head: copyright-comma
created_at: 2024-01-12T21:17:41Z
updated_at: 2024-03-22T18:43:37Z
url: https://github.com/astral-sh/ruff/pull/9498
synced_at: 2026-01-10T22:47:01Z
```

# Accept commas in default copyright pattern

---

_Pull request opened by @dopplershift on 2024-01-12 21:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds commas as an accepted separator between copyright years by default, which is actually documented in one spot, but not currently accurate. Fixes #9477.

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-12 22:57_

---

_Comment by @charliermarsh on 2024-01-13 02:49_

Just trying to add tests for this... It seems like `Copyright (c) 2018, 2019 Developers` is actually accepted on main? See: https://play.ruff.rs/5c5932af-5fef-4990-bcc5-c854c0b46255

---

_Comment by @charliermarsh on 2024-01-13 02:50_

It seems like the current regex could be described as "Must contain `Copyright\s+(\(C\)\s+)?\d{4}`, followed by anything". So `Copyright (c) 2018, 2019 Developers` would match as "`Copyright (c) 2018`, followed by anything".

---

_Comment by @BurntSushi on 2024-01-13 04:24_

> It seems like the current regex could be described as "Must contain `Copyright\s+(\(C\)\s+)?\d{4}`, followed by anything". So `Copyright (c) 2018, 2019 Developers` would match as "`Copyright (c) 2018`, followed by anything".

Nit: you probably want `[0-9]` instead of `\d`. Or, equivalently, `(?-u:\d)`. See: https://docs.rs/regex/latest/regex/#example-validating-a-particular-date-format

---

_Comment by @charliermarsh on 2024-01-24 01:57_

Closing for now, but happy to revisit if we get more info.

---

_Closed by @charliermarsh on 2024-01-24 01:57_

---

_Reopened by @charliermarsh on 2024-03-22 18:13_

---

_@charliermarsh approved on 2024-03-22 18:28_

---

_Comment by @github-actions[bot] on 2024-03-22 18:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-22 18:42_

---

_Closed by @charliermarsh on 2024-03-22 18:42_

---

_Label `bug` added by @charliermarsh on 2024-03-22 18:42_

---

_Branch deleted on 2024-03-22 18:43_

---
