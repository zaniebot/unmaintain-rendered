```yaml
number: 7883
title: "fix(schema): restore limit on line-length"
type: pull_request
state: merged
author: henryiii
labels: []
assignees: []
merged: true
base: main
head: henryiii/fix/schemaminmax
created_at: 2023-10-10T02:39:17Z
updated_at: 2023-10-10T03:36:04Z
url: https://github.com/astral-sh/ruff/pull/7883
synced_at: 2026-01-12T02:32:41Z
```

# fix(schema): restore limit on line-length

---

_Pull request opened by @henryiii on 2023-10-10 02:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Restores functionality of #7875 but in the correct place. Closes #7877.

~~I couldn't figure out how to get cargo fmt to work, so hopefully that's run in CI.~~ Nevermind, figured it out.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Can see output of json.

<!-- How was it tested? -->


---

_@henryiii reviewed on 2023-10-10 02:41_

---

_Review comment by @henryiii on `crates/ruff_linter/src/line_width.rs`:20 on 2023-10-10 02:41_

```suggestion
#[cfg_attr(feature = "schemars", schemars(range(min = 1, max = 320)))] NonZeroU16
```

---

_Comment by @github-actions[bot] on 2023-10-10 03:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-10 03:03_

Ah excellent! Thank you @henryiii!

---

_Merged by @charliermarsh on 2023-10-10 03:04_

---

_Closed by @charliermarsh on 2023-10-10 03:04_

---

_Branch deleted on 2023-10-10 03:04_

---

_Comment by @zanieb on 2023-10-10 03:36_

Thanks!!

---
