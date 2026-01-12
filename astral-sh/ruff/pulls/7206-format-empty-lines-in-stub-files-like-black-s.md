```yaml
number: 7206
title: "Format empty lines in stub files like black's preview style"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: typeshed-empty-line-rules
created_at: 2023-09-06T17:17:53Z
updated_at: 2023-09-11T08:04:01Z
url: https://github.com/astral-sh/ruff/pull/7206
synced_at: 2026-01-12T02:45:38Z
```

# Format empty lines in stub files like black's preview style

---

_Pull request opened by @konstin on 2023-09-06 17:17_

## Summary

Fix all but one empty line differences with the black preview style in typeshed. The remaining differences are breaking with type comments and trailing commas in function definitions.

I compared the empty line differences with the preview mode of black since stable has some oddities that would have been hard to replicate (https://github.com/psf/black/issues/3861). Additionally, it assumes the style proposed in https://github.com/psf/black/issues/3862.

An edge case that also surfaced with typeshed are newline before trailing module comments.

**main**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99966 |              2760 |                58 |
| transformers |           0.99930 |              2587 |               447 |
| twine        |           1.00000 |                33 |                 0 |
| **typeshed**     |           0.99978 |              3496 |              **2173** |
| warehouse    |           0.99825 |               648 |                22 |
| zulip        |           0.99950 |              1437 |                27 |

**PR**
| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99966 |              2760 |                58 |
| transformers |           0.99930 |              2587 |               447 |
| twine        |           1.00000 |                33 |                 0 |
| **typeshed**     |           0.99983 |              3496 |                **18** |
| warehouse    |           0.99825 |               648 |                22 |
| zulip        |           0.99950 |              1437 |                27 |


Closes #6723

## Test Plan

The main driver was the typeshed diff. I added new test cases for all kinds of possible empty line combinations in stub files, test cases for newlines before trailing module comments.

---

_Converted to draft by @konstin on 2023-09-06 17:18_

---

_Label `formatter` added by @konstin on 2023-09-06 17:18_

---

_Renamed from "Empty line rules for stub files" to "Format empty lines in stub files closer to black's preview style" by @konstin on 2023-09-08 15:18_

---

_@konstin reviewed on 2023-09-08 15:26_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:135 on 2023-09-08 15:26_

it's a bit counterintuitive that the exceptions are the named cases

---

_Marked ready for review by @konstin on 2023-09-08 16:29_

---

_Renamed from "Format empty lines in stub files closer to black's preview style" to "Format empty lines in stub files like black's preview style" by @konstin on 2023-09-08 16:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:135 on 2023-09-11 07:28_

Should `empty_lines` be inlined into `empty_lines_before_trailing_comments`. It seems the function is only used in `stmt_function_def` and `stmt_class_def` and both use the same implementation.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:366 on 2023-09-11 07:32_

Can we make the formatter readonly. The function doesn't seem to call into `fmt`
```suggestion
fn stub_suite_can_omit_empty_line(preceding: &Stmt, following: &Stmt, f: &PyFormatter) -> bool {
```

---

_@MichaReiser approved on 2023-09-11 07:33_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:366 on 2023-09-11 07:44_

huh thanks good catch, strange that clippy didn't pick that up

---

_@konstin reviewed on 2023-09-11 07:44_

---

_Merged by @konstin on 2023-09-11 08:04_

---

_Closed by @konstin on 2023-09-11 08:04_

---

_Branch deleted on 2023-09-11 08:04_

---
