```yaml
number: 15387
title: Parse triple quoted string annotations as if parenthesized
type: pull_request
state: merged
author: Glyphack
labels:
  - parser
assignees: []
merged: true
base: main
head: stringized-annotations
created_at: 2025-01-09T23:37:26Z
updated_at: 2025-05-21T20:26:37Z
url: https://github.com/astral-sh/ruff/pull/15387
synced_at: 2026-01-12T15:55:51Z
```

# Parse triple quoted string annotations as if parenthesized

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Resolves #9467 

Parse quoted annotations as if the string content is inside parenthesis.
With this logic `x` and `y` in this example are equal:

```python
y: """
   int |
   str
"""

z: """(
    int |
    str
)
"""
```

Also this rule only applies to triple quotes([link](https://github.com/python/typing-council/issues/9#issuecomment-1890808610)).

This PR is based on the [comments](https://github.com/astral-sh/ruff/issues/9467#issuecomment-2579180991) on the issue.

I did one extra change, since we don't want any indentation tokens I am setting the `State::Other` as the initial state of the Lexer.

Remaining work:

- [x] Add a test case for red-knot.
- [x] Add more tests.

## Test Plan

Added a test which previously failed because quoted annotation contained indentation.
Added an mdtest for red-knot.
Updated previous test.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-01-09 23:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_@Glyphack reviewed on 2025-01-13 22:34_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/src/lib.rs`:195 on 2025-01-13 22:34_

I don't know if this function is necessary. I was worried someone might update the linter parsing logic for string annotations and forget red knot. With this at least it will be applied for both.

---

_Marked ready for review by @Glyphack on 2025-01-13 22:39_

---

_Review requested from @carljm by @Glyphack on 2025-01-13 22:39_

---

_Review requested from @MichaReiser by @Glyphack on 2025-01-13 22:39_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-01-13 22:39_

---

_Review requested from @sharkdp by @Glyphack on 2025-01-13 22:39_

---

_Review requested from @dhruvmanila by @Glyphack on 2025-01-13 22:39_

---

_Renamed from "Parse Quoted annotations as if parenthesized" to "Parse triple quoted annotations as if parenthesized" by @Glyphack on 2025-01-13 22:44_

---

_Renamed from "Parse triple quoted annotations as if parenthesized" to "Parse triple quoted string annotations as if parenthesized" by @Glyphack on 2025-01-13 22:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:198 on 2025-01-13 23:48_

I assume out of these four, only `a4` failed before this PR? (Can't check right now, since GH git ops are down.)

Not sure what `a1` and `a2` are really testing?

---

_@carljm reviewed on 2025-01-13 23:50_

Thank you!! The red-knot test and changes look fine to me, but that's a small part of this PR. Would rather have @dhruvmanila take a look at the parser/ruff side.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:645 on 2025-01-14 07:34_

I'd move it next to `Expression` and let's add a comment explaining what this mode is.

---

_@MichaReiser reviewed on 2025-01-14 07:34_

Thank you. Would you mind adding a test where an expression is incorrectly parenthesized? 

```python
def f(
	a: """
		int | 
str)
"""
)
```

or 

```python
def f(
	a: """
		int) | 
str
"""
)
```

I'm curious how it recovers in those cases and if we need to do something more than just setting `nesting = 1` 

---

_Label `parser` added by @MichaReiser on 2025-01-14 07:34_

---

_@Glyphack reviewed on 2025-01-14 09:03_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:198 on 2025-01-14 09:03_

Yes only a4 fails. I initially added the other ones to test more cases. But I now replaced those with the better cases that Micha [provided](https://github.com/astral-sh/ruff/pull/15387#pullrequestreview-2548984289)

---

_Comment by @Glyphack on 2025-01-14 18:45_

@MichaReiser I added the test cases you suggested and the tests pass(see this [commit](https://github.com/astral-sh/ruff/pull/15387/commits/b3cf440dfdc9500afd4826b23d97363928cb9ec4))
But I think that is because the parser is recovering.

So I fixed the nesting check to check for nesting value of 1 when mode is `ParenthesizedExpression`. This way the lexer will detect the problem that nesting is one level lower than started as well.


---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:91 on 2025-01-15 05:22_

nit:
```suggestion
        let (state, nesting) = if mode == Mode::ParenthesizedExpression {
            (State::Other, 1)
        } else {
            (State::AfterNewline, 0)
        };
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:649 on 2025-01-15 05:27_

I've suggested some improvements and I don't think we should specify the internal details about implementation within the lexer.

```suggestion
    /// The code consists of a single expression and is parsed as if it is parenthesized. The parentheses themselves aren't required.
    /// This allows for having valid multiline expression without the need of parentheses
    /// and is specifically useful for parsing string annotations.
    ParenthesizedExpression,
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:195 on 2025-01-15 05:28_

What do you think about `parse_string_annotation` or `parse_string_annotation_content`? Additionally, we should provide documentation similar to what you've done in `parse_parenthesized_expression_range` as this is a public function and the crate is being used by multiple projects.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pyflakes/F722.py`:29 on 2025-01-15 05:33_

Can we add the same test case using triple-quoted string which will be useful as a documentation?

```py
# but, using the same annotation inside a triple-quoted string is valid
valid: """\n int"""
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pyflakes/F722.py`:38 on 2025-01-15 05:41_

Can we add some test cases for unclosed parenthesis similar to how these? It would also be useful to provide test cases with multiple parentheses.

---

_@dhruvmanila reviewed on 2025-01-15 05:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1328 on 2025-01-15 05:53_

Should this be `if self.nesting > 1`?

---

_@dhruvmanila reviewed on 2025-01-15 05:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1337 on 2025-01-15 07:34_

```suggestion
				// For Mode::ParenthesizedExpression we start with nesting level 1.
        // So we check if we end with that level.
				let init_nesting = if self.mode == Mode::ParenthesizedExpression {
					1
				} else {
					0
				};

        if self.nesting > init_nesting {
            // Reset the nesting to avoid going into infinite loop.
            self.nesting = 0;
            return self
                .push_error(LexicalError::new(LexicalErrorType::Eof, self.token_range()));
        }
            
```

---

_@MichaReiser approved on 2025-01-15 07:36_

Thanks. This looks great. 

---

_@Glyphack reviewed on 2025-01-15 19:01_

---

_Review comment by @Glyphack on `crates/ruff_linter/resources/test/fixtures/pyflakes/F722.py`:29 on 2025-01-15 19:01_

Actually I made a mistake here, `invalid: "\n int"` is in a python file. So this string will not actually have a newline character but `\` and `n` as characters. So also `""""\n int"""` fails.
Is there a way to actually create a type annotation that contains newline inside a python file without using triple quotes?

https://pyright-play.net/?code=JYOwbghgNsAmBcACARKgOiRoAurkCh9RIYEVUiRcKg

---

_@Glyphack reviewed on 2025-01-15 20:41_

---

_Review comment by @Glyphack on `crates/ruff_linter/resources/test/fixtures/pyflakes/F722.py`:29 on 2025-01-15 20:41_

I removed this right now. Since I could not figure out how to do it correctly.

---

_@Glyphack reviewed on 2025-01-15 20:41_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/src/lexer.rs`:1328 on 2025-01-15 20:41_

I applied the suggestion below and it resolves it.

---

_@Glyphack reviewed on 2025-01-15 20:42_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/src/lib.rs`:195 on 2025-01-15 20:42_

I agree, renamed to `parse_string_annotation` and added documentation.

---

_Review requested from @dhruvmanila by @Glyphack on 2025-01-15 20:44_

---

_@dhruvmanila reviewed on 2025-01-16 06:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pyflakes/F722.py`:29 on 2025-01-16 06:03_

I see, that makes sense. I don't think it's even possible to have a newline in a single-quoted string without the `\n` character. This will also utilize `parse_complex_annotation` and not `parse_simple_annotation`.

---

_@dhruvmanila reviewed on 2025-01-16 06:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F722_F722.py.snap`:23 on 2025-01-16 06:06_

Unrelated to this PR, I think it would be useful to provide the syntax error that was raised during parsing this string content.

---

_@dhruvmanila approved on 2025-01-16 06:07_

Thank you for working on this!

---

_Merged by @dhruvmanila on 2025-01-16 06:08_

---

_Closed by @dhruvmanila on 2025-01-16 06:08_

---

_Branch deleted on 2025-05-21 20:26_

---
