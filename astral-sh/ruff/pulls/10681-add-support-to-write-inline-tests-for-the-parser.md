```yaml
number: 10681
title: Add support to write inline tests for the parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/inline-tests
created_at: 2024-03-31T10:36:49Z
updated_at: 2024-04-02T12:10:28Z
url: https://github.com/astral-sh/ruff/pull/10681
synced_at: 2026-01-12T15:55:32Z
```

# Add support to write inline tests for the parser

---

_@dhruvmanila_

## Summary

This PR adds support to write inline tests for the parser.

Inline tests are defined in the Rust code itself. The test are written through comments of a specific form:
```rs
// test this_is_the_test_name
// def foo():
//     pass
println!("some rust code");

// test_err this_test_should_raise_syntax_err
// [1, 2
println!("some rust code");
```

This way it's easier to reason about a specific code in the parser and the tests are used both as an example and a way to test that logic.

This code runs as an integration tests which fails in the following cases:
1. When there are test comments but no corresponding test files

	```
	running 1 test
	Error: Following files were not up-to date and has been updated:
	  crates/ruff_python_parser/resources/inline/ok/slice_single_starred_element.py
	  crates/ruff_python_parser/resources/inline/ok/slice_single_element.py
	Re-run the tests with `cargo test`
	
	test generate_inline_tests ... FAILED
	```

2. When there are test files but no corresponding test comments

	```
	running 1 test
	Error: Unreferenced test files found for which no comment exists:
	  crates/ruff_python_parser/resources/inline/ok/slice_single_element.py
	  crates/ruff_python_parser/resources/inline/ok/slice_single_starred_element.py
	
	test generate_inline_tests ... FAILED
	```

3. When there are multiple tests with the same name

	```
	running 1 test
	Error: Duplicate test found: "slice_single_element" (search '// test slice_single_element' for the location)

	test generate_inline_tests ... FAILED
	```

## References:

- `rust-analyzer`: https://github.com/rust-lang/rust-analyzer/blob/e4a405f877efd820bef9c0e77a02494e47c17512/crates/parser/src/tests/sourcegen_inline_tests.rs
- `biome`: https://github.com/biomejs/biome/blob/b9f8ffea9967b098ec4c8bf74fa96826a879f043/xtask/codegen/src/parser_tests.rs

## Test Plan

Using the below code:
```rs
// test slice_single_starred_element
// x[*y]

// test slice_single_element
// x[y]
```

Run the new tests with:
```
cargo test --package ruff_python_parser --test generate_inline_tests
```


---

_Label `parser` added by @dhruvmanila on 2024-03-31 10:36_

---

_Label `testing` added by @dhruvmanila on 2024-03-31 10:36_

---

_Comment by @github-actions[bot] on 2024-03-31 10:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/8679e99468d75807cffa3124f9ee2933af75f643/stdlib/unittest/mock.pyi#L356'>stdlib/unittest/mock.pyi:356:1:</a> E999 SyntaxError: unexpected EOF while parsing
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-04-01 04:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-01 04:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/generate_inline_tests.rs`:57 on 2024-04-02 07:54_

Can we extend the error message with what needs to be done in this case? Is there a specific command that i need to run? Should I delete the test?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/generate_inline_tests.rs`:78 on 2024-04-02 07:55_

```suggestion
            "Following files were not up-to date and have been updated:\n  {}\nRe-run the tests with `cargo test` to update the test snapshots.\n",
```

---

_@MichaReiser approved on 2024-04-02 07:57_

I think it would be good to extend the `CONTRIBUTING.md` and mention the parser inline tests (or document them as part of the parser readme file?)

---

_Comment by @dhruvmanila on 2024-04-02 10:46_

> I think it would be good to extend the `CONTRIBUTING.md` and mention the parser inline tests (or document them as part of the parser readme file?)

I'm going to mark this as a TODO for later as I think we should have a README for the parser.

---

_Comment by @MichaReiser on 2024-04-02 10:50_

> I'm going to mark this as a TODO for later as I think we should have a README for the parser.

I think having a small note in the README even if the readme itself isn't complete would be useful. E.g. it wasn't clear to me when reading the PR how I would run and update the tests. 

---

_Comment by @dhruvmanila on 2024-04-02 11:22_

@MichaReiser I added a basic contributing doc containing a section about inline tests.

---

_@dhruvmanila reviewed on 2024-04-02 11:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/generate_inline_tests.rs`:57 on 2024-04-02 11:23_

Updated the message with "Please delete these files manually"

---

_Merged by @dhruvmanila on 2024-04-02 12:10_

---

_Closed by @dhruvmanila on 2024-04-02 12:10_

---

_Branch deleted on 2024-04-02 12:10_

---
