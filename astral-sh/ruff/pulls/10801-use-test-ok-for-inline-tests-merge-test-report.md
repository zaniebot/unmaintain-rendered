```yaml
number: 10801
title: "Use `test_ok` for inline tests, merge test report"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/merge-inline-tests
created_at: 2024-04-06T05:25:35Z
updated_at: 2024-04-06T05:51:20Z
url: https://github.com/astral-sh/ruff/pull/10801
synced_at: 2026-01-12T15:55:33Z
```

# Use `test_ok` for inline tests, merge test report

---

_@dhruvmanila_

## Summary

This PR makes two quality of life improvements for parser's inline test implementation:
1. Rename `test` to `test_ok` to explicitly specify that this is a valid syntax test
2. Merge the reporting of "ok" and "err" test cases and unreferenced test cases

The output of (2) now looks like:

```
---- generate_inline_tests stdout ----
Error: Unreferenced test files found for which no comment exists:
  crates/ruff_python_parser/resources/inline/err/something_else.py
Please delete these files manually

Following files were not up-to date and has been updated:
  crates/ruff_python_parser/resources/inline/err/renamed_it.py
Re-run the tests with `cargo test` to update the test snapshots

```

## Test Plan

1. Add two random test cases:

	```rs
	// test_ok something
	// x = 1
	
	// test_err something_else
	// [1, 2
	```

2. Run the generation step:

	```console
	cargo test --package ruff_python_parser --test generate_inline_tests
	```

3. Rename `something_else` to `renamed_it` and run the generation step again to see the above output

---

_Label `internal` added by @dhruvmanila on 2024-04-06 05:25_

---

_Label `parser` added by @dhruvmanila on 2024-04-06 05:25_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-06 05:25_

---

_Renamed from "Use `test_ok`, merge test report" to "Use `test_ok` for inline tests, merge test report" by @dhruvmanila on 2024-04-06 05:30_

---

_Review comment by @dhruvmanila on `.gitattributes`:12 on 2024-04-06 05:33_

I'm not sure if this will work unless it's on `main`, so let's see.

---

_@dhruvmanila reviewed on 2024-04-06 05:33_

---

_Merged by @dhruvmanila on 2024-04-06 05:33_

---

_Closed by @dhruvmanila on 2024-04-06 05:33_

---

_Branch deleted on 2024-04-06 05:33_

---

_Comment by @github-actions[bot] on 2024-04-06 05:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
