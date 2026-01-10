```yaml
number: 22405
title: Update Black tests
type: pull_request
state: merged
author: dylwil3
labels:
  - formatter
  - testing
assignees: []
merged: true
base: main
head: update-black-fixtures
created_at: 2026-01-05T17:58:07Z
updated_at: 2026-01-06T15:09:08Z
url: https://github.com/astral-sh/ruff/pull/22405
synced_at: 2026-01-10T16:30:32Z
```

# Update Black tests

---

_Pull request opened by @dylwil3 on 2026-01-05 17:58_

I am updating these because we didn't have test coverage for the different handling of `fmt: skip` comments applied to multiple statements on the same line. This is in preparation for #22119 (to show before/after deviations).

Follows the same procedure as in #20794

Edit: As it happens, the new fixtures do not even cover the case relevant to #22119 - they just deal with the already handled case of a one-line compound statement. Nevertheless, it seems worthwhile to make this update, especially since it uncovered a bug.


---

_Label `internal` added by @dylwil3 on 2026-01-05 17:58_

---

_Label `internal` removed by @dylwil3 on 2026-01-05 17:58_

---

_Label `formatter` added by @dylwil3 on 2026-01-05 17:58_

---

_Label `testing` added by @dylwil3 on 2026-01-05 17:58_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip10.py.snap`:75 on 2026-01-05 18:09_

These empty lines look like a bug to me, presumably in the implementation of fmt skip for clauses. I will open an issue.

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 18:10_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtonoff.py.snap`:245 on 2026-01-05 18:11_

I think Ruff is getting it right here

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip11.py.snap`:110 on 2026-01-05 18:15_

Known deviation - we don't support off/on within an expression.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip12.py.snap`:24 on 2026-01-05 18:19_

Known deviation: we format the skip comments

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip13.py.snap`:51 on 2026-01-05 18:20_

Known deviation: suppressing inside an expression

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip7.py.snap`:23 on 2026-01-05 18:21_

Known deviation: we format the suppression comment

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip_multiple_in_clause.py.snap`:49 on 2026-01-05 18:23_

I think this is another instance of suppressions within expressions.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip_multiple_strings.py.snap`:63 on 2026-01-05 18:24_

Known deviation: suppression within expression

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep_750.py.snap`:53 on 2026-01-05 18:29_

I believe these are all equivalent to the known deviations in f-string formatting.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_fmtpass_imports.py.snap`:41 on 2026-01-05 18:30_

This is expected because this option for import sorting is covered by the linter and not the formatter (at the moment)

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_standardize_type_comments.py.snap`:38 on 2026-01-05 18:31_

Expected since we have not implemented this preview style

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__remove_parens_from_lhs.py.snap`:44 on 2026-01-05 18:32_

Known deviation

---

_@dylwil3 reviewed on 2026-01-05 18:33_

---

_@dylwil3 reviewed on 2026-01-05 18:41_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip10.py.snap`:75 on 2026-01-05 18:41_

opened: #22406

---

_Marked ready for review by @dylwil3 on 2026-01-05 18:42_

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-05 18:42_

---

_@MichaReiser approved on 2026-01-06 09:02_

---

_Merged by @dylwil3 on 2026-01-06 15:09_

---

_Closed by @dylwil3 on 2026-01-06 15:09_

---

_Branch deleted on 2026-01-06 15:09_

---
