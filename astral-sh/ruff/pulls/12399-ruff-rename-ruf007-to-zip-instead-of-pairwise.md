```yaml
number: 12399
title: "[`ruff`] Rename `RUF007` to `zip-instead-of-pairwise`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: rename-ruf007
created_at: 2024-07-18T23:22:07Z
updated_at: 2024-07-20T19:17:33Z
url: https://github.com/astral-sh/ruff/pull/12399
synced_at: 2026-01-10T21:47:02Z
```

# [`ruff`] Rename `RUF007` to `zip-instead-of-pairwise`

---

_Pull request opened by @dylwil3 on 2024-07-18 23:22_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Renames the rule [RUF007](https://docs.astral.sh/ruff/rules/pairwise-over-zipped/) from `pairwise-over-zipped` to `zip-instead-of-pairwise`. This closes #12397. 

Specifically, in this PR:

- The file containing the rule was renamed
- The struct was renamed
- The function implementing the rule was renamed

## Testing

<!-- How was it tested? -->

- `cargo test`
- Docs re-built locally and verified that new rule name is displayed. (Screenshots below).

<img width="939" alt="New rule name in rule summary" src="https://github.com/user-attachments/assets/bf638bc9-1b7a-4675-99bf-e4de88fec167">

<img width="805" alt="New rule name in rule details" src="https://github.com/user-attachments/assets/6fffd745-2568-424a-84e5-f94a41351022">




---

_Renamed from "Rename ruf007" to "[`ruff`] Rename `RUF007` to `zip-instead-of-pairwise`" by @charliermarsh on 2024-07-18 23:26_

---

_Label `rule` added by @charliermarsh on 2024-07-18 23:26_

---

_Comment by @charliermarsh on 2024-07-18 23:26_

Thanks!

---

_Merged by @charliermarsh on 2024-07-18 23:26_

---

_Closed by @charliermarsh on 2024-07-18 23:26_

---

_Branch deleted on 2024-07-18 23:27_

---

_Comment by @hauntsaninja on 2024-07-20 18:40_

Maybe my English is off, but shouldn't this be pairwise-instead-of-zip ?

---

_Comment by @charliermarsh on 2024-07-20 19:17_

I think this is correct, because the convention is such that "allow zip-instead-of-pairwise" should make sense (given that we're recommending pairwise to the user).

---
