```yaml
number: 7835
title: "Add hidden `--parser` option to override the input format, rather than inferring it from the file extension"
type: pull_request
state: closed
author: felix-cw
labels: []
assignees: []
base: main
head: felix-cw/cli-parser-option
created_at: 2023-10-06T10:00:23Z
updated_at: 2023-10-31T10:48:04Z
url: https://github.com/astral-sh/ruff/pull/7835
synced_at: 2026-01-12T15:55:25Z
```

# Add hidden `--parser` option to override the input format, rather than inferring it from the file extension

---

_@felix-cw_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This adds a hidden `--parser` option to the `check` command, motivated by #6847. 
This option allows overriding the input format of the code, rather than inferring from the file extension.

The possible values are `py` and `ipynb`. If `--parser` is not specified, file formats are inferred from file extensions.

## Test Plan

- Added tests in `integration_tests.rs` to ensure the parser override worked as expected.



---

_Comment by @github-actions[bot] on 2023-10-06 10:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-20 16:20_

This is really nice work. Let me try to get some alignment on what we want this argument to be called in the originating issue, since there was some discussion there still...

---

_Review requested from @charliermarsh by @charliermarsh on 2023-10-20 16:20_

---

_Comment by @charliermarsh on 2023-10-20 16:21_

(Assigning myself as reviewer.)

---

_Comment by @felix-cw on 2023-10-23 14:36_

Thanks for the kind comment! I'm thinking that the preferred idea in the discussion is to provide an option that maps file extensions to language, whereas this just overrides the language for everything. Please don't hesitate to close this PR if this is the wrong approach.

---

_Comment by @felix-cw on 2023-10-31 10:48_

I'm closing this PR in favour of  #8373 which seems to be a more sensible approach.

---

_Closed by @felix-cw on 2023-10-31 10:48_

---
