```yaml
number: 1501
title: "Adjust `test_path` helper to detect round-trip autofix issues"
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: round-trip-fix-test
created_at: 2022-12-31T08:00:18Z
updated_at: 2022-12-31T13:02:39Z
url: https://github.com/astral-sh/ruff/pull/1501
synced_at: 2026-01-12T05:36:31Z
```

# Adjust `test_path` helper to detect round-trip autofix issues

---

_Pull request opened by @squiddy on 2022-12-31 08:00_

This correctly detects the issues described in #1490. I'll leave that up to you whether to pull this in or not and will prepare the fix as a separate PR.

```console
failures:

---- isort::tests::default::path_new_line_ending_crlf_py_expects stdout ----
thread 'isort::tests::default::path_new_line_ending_crlf_py_expects' panicked at 'Failed to converge after 100 iterations. This likely indicates a bug in the implementation of the fix.', src/linter.rs:557:21
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- isort::tests::default::path_new_line_ending_cr_py_expects stdout ----
thread 'isort::tests::default::path_new_line_ending_cr_py_expects' panicked at 'Failed to converge after 100 iterations. This likely indicates a bug in the implementation of the fix.', src/linter.rs:557:21


failures:
    isort::tests::default::path_new_line_ending_cr_py_expects
    isort::tests::default::path_new_line_ending_crlf_py_expects

test result: FAILED. 745 passed; 2 failed; 3 ignored; 0 measured; 0 filtered out; finished in 0.41s
```

---

_Merged by @charliermarsh on 2022-12-31 13:02_

---

_Closed by @charliermarsh on 2022-12-31 13:02_

---

_Branch deleted on 2022-12-31 13:02_

---
