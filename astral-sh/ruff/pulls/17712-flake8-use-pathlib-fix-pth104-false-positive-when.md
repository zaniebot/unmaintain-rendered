```yaml
number: 17712
title: "[`flake8-use-pathlib`] Fix `PTH104`false positive when `rename` is passed a file descriptor"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-PTH104
created_at: 2025-04-29T14:50:07Z
updated_at: 2025-05-01T14:01:18Z
url: https://github.com/astral-sh/ruff/pull/17712
synced_at: 2026-01-10T18:57:03Z
```

# [`flake8-use-pathlib`] Fix `PTH104`false positive when `rename` is passed a file descriptor

---

_Pull request opened by @LaBatata101 on 2025-04-29 14:50_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Contains the same changes to the semantic type inference as https://github.com/astral-sh/ruff/pull/17705.

Fixes #17694
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Snapshot tests.


---

_Comment by @github-actions[bot] on 2025-04-29 14:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-04-30 06:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:45 on 2025-04-30 06:45_

Can we add a test where the `src_dir_fd` is specified by position (and not name) and another test where `src_dir_fd` is `None`. The same for `dst_dir_fd`

---

_@MichaReiser reviewed on 2025-04-30 06:45_

---

_Label `bug` added by @MichaReiser on 2025-04-30 06:45_

---

_@LaBatata101 reviewed on 2025-04-30 14:13_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:45 on 2025-04-30 14:13_

`src_dir_fd` and `dst_dir_fd` are keyword only parameters, can't be specified by position

```
os.rename(src, dst, *, src_dir_fd=None, dst_dir_fd=None)
```

[https://docs.python.org/3/library/os.html#os.rename](https://docs.python.org/3/library/os.html#os.rename)

---

_@LaBatata101 reviewed on 2025-04-30 14:28_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:45 on 2025-04-30 14:28_

I've added the tests you mentioned.

---

_@MichaReiser approved on 2025-05-01 06:16_

Thank you. Lets remove the bytes string handling for now (sorry for that)

---

_Renamed from "[`flake8-use-pathlib`] Fix `PTH104`false positive when `rename` is passed a file descriptor or bytes string" to "[`flake8-use-pathlib`] Fix `PTH104`false positive when `rename` is passed a file descriptor" by @LaBatata101 on 2025-05-01 13:17_

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-05-01 13:37_

---

_Comment by @ntBre on 2025-05-01 13:43_

Oh thanks for opening the other PR! We were preparing for the release today and I just pushed similar changes here. I opted to leave the test cases, but otherwise they look very similar.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__full_name.py.snap`:321 on 2025-05-01 13:53_

I haven't looked at the issue more closely, but does the handling of bytes string is planned as a follow-up or it's to be avoided for now. Given that the tests are not removed, I'm guessing the former. If so, it'd be useful to add a TODO (or possibly remove the tests.)

---

_@dhruvmanila approved on 2025-05-01 13:53_

---

_@MichaReiser reviewed on 2025-05-01 13:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__full_name.py.snap`:321 on 2025-05-01 13:55_

We want to keep flagging them. So an exmaple seems fine.

---

_@ntBre reviewed on 2025-05-01 13:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__full_name.py.snap`:321 on 2025-05-01 13:57_

There was some discussion starting [here](https://github.com/astral-sh/ruff/issues/17699#issuecomment-2840253469) about possible handling of bytes, and I think these are the only bytes tests in the whole file, so I leaned toward keeping them. Happy to delete them if you prefer, though.

---

_@dhruvmanila reviewed on 2025-05-01 14:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__full_name.py.snap`:321 on 2025-05-01 14:00_

Fine to keep them.

---

_Merged by @ntBre on 2025-05-01 14:01_

---

_Closed by @ntBre on 2025-05-01 14:01_

---
