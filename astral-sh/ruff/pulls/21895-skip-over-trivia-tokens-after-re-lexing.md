```yaml
number: 21895
title: Skip over trivia tokens after re-lexing
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: micha/parser-trivia-relex
created_at: 2025-12-10T14:10:26Z
updated_at: 2025-12-12T06:13:22Z
url: https://github.com/astral-sh/ruff/pull/21895
synced_at: 2026-01-10T16:42:11Z
```

# Skip over trivia tokens after re-lexing

---

_Pull request opened by @MichaReiser on 2025-12-10 14:10_

## Summary

Unlike the `Lexer`, the `TokenSource` skips over trivia token because they aren't semantically meaningful
during parsing. However, this hasn't been the case after re-lexing where we forgot to skip over any trivia, 
which this PR adds

Fixes https://github.com/astral-sh/ty/issues/1828

## Test Plan

This PR extends our fixture to assert that a node start or end falls within a token range (because that woudl be weird). 
I added regression tests for the two fixed bugs.

I'm not super happy with how the parser recovers but I'll create a separate PR with some discussion around that (I don't have a solution yet).



---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-10 14:10_

---

_Label `bug` added by @MichaReiser on 2025-12-10 14:10_

---

_Label `parser` added by @MichaReiser on 2025-12-10 14:10_

---

_@dylwil3 reviewed on 2025-12-10 14:12_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/resources/valid/expressions/21538.py`:2 on 2025-12-10 14:12_

This test already passes on `main`. The panic in that issue is caused because RUF027 calls `parse_expression` on `f"{'bar': {\t'"`.

---

_@MichaReiser reviewed on 2025-12-10 14:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lex_logical_token.py.snap`:836 on 2025-12-10 14:12_

I find those error messages more accurate. We only expect a `]` because `def` is the start of a new keyword. It's otherwise perfectly fine to write a list over multiple lines.

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 14:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 14:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-10 14:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lex_logical_token_mac_eol.py.snap`:122 on 2025-12-10 14:15_

I think this is bad but it's inherent to how we do recovery. The issue is that `nesting` is still greater than 1 when we're recovering in `[` so that the `NonLogicalNewline` isn't relexed as a `Newline`. However, it is later re-lexed when we recover from the unclosed `(`, which feels a bit inconsistent.

---

_@MichaReiser reviewed on 2025-12-10 14:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lexing__ty_1828.py.snap`:226 on 2025-12-10 14:16_

This error message is just confusing because the class itself is fine. But it's because of a previous error.

---

_@MichaReiser reviewed on 2025-12-10 14:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lexing__ty_1828.py.snap`:199 on 2025-12-10 14:17_

This is also just funny but it's the same problem. The recovery in f-string has a nesting level > 0, so it doesn't actually do anything.

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 14:21_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected `)`, found newline
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected `}`, found NonLogicalNewline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:12:5: invalid-syntax: Expected `}`, found `return`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 3 | 2 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected `)`, found newline
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected `}`, found NonLogicalNewline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:12:5: invalid-syntax: Expected `}`, found `return`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 3 | 2 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:70 on 2025-12-11 05:27_

nit: `non_logical_newline` ?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:123 on 2025-12-11 05:33_

Is this the main part of the fix based on the PR description?

---

_@dhruvmanila approved on 2025-12-11 05:33_

---

_@dhruvmanila reviewed on 2025-12-11 05:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lex_logical_token.py.snap`:836 on 2025-12-11 05:34_

Hmm, but then the following error message points at this location "expecting `)`, found `Newline`". I think this is still an improvement but just found this a bit inconsistent. No need to change anything here.

---

_@dhruvmanila reviewed on 2025-12-11 05:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lexing__ty_1828.py.snap`:199 on 2025-12-11 05:36_

Probably related to https://github.com/astral-sh/ruff/issues/11946

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:123 on 2025-12-11 08:25_

Yes, the other changes are mainly added assertions and some safe-guarding that we never remove any tokens that came before the non logical newline

---

_@MichaReiser reviewed on 2025-12-11 08:25_

---

_Merged by @MichaReiser on 2025-12-11 10:45_

---

_Closed by @MichaReiser on 2025-12-11 10:45_

---

_Branch deleted on 2025-12-11 10:45_

---

_Comment by @dhruvmanila on 2025-12-12 05:20_

Do we need to change anything in `TokenIterWithContext` here: https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ruff_python_ast/src/token/tokens.rs#L300-L305

which tries to "mimic" this behavior of recovery. I think no, but just wanted to check-in with you.

---

_Comment by @MichaReiser on 2025-12-12 06:13_

I don't think so

---
