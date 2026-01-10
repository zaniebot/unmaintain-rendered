```yaml
number: 10293
title: "Remove `skip_until` parser method"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/deprecate-skip-until
created_at: 2024-03-08T06:52:02Z
updated_at: 2024-03-08T17:58:09Z
url: https://github.com/astral-sh/ruff/pull/10293
synced_at: 2026-01-10T22:47:01Z
```

# Remove `skip_until` parser method

---

_Pull request opened by @dhruvmanila on 2024-03-08 06:52_

## Summary

This PR removes the `skip_until` parser method. The main use case for it was for error recovery which we want to isolate only in list parsing.

There are two references which are removed:
1. Parsing a list of match arguments in a class pattern. Take the following code snippet as an example:

	```python
	match foo:
		case Foo(bar.z=1, baz):
			pass
	```
	This is a syntax error as the keyword argument pattern can only have an identifier but here it's an attribute node. Now, to move on to the next argument (`baz`), the parser would skip until the end of the argument to recover. What we will do now is to parse the value as a pattern (per spec) thus moving the parser ahead and add the node with an empty identifier.

	The above code will produce the following AST:

	<details><summary><b>AST</b></summary>
	<p>
	
	```rs
	Module(
	    ModModule {
	        range: 0..52,
	        body: [
	            Match(
	                StmtMatch {
	                    range: 0..51,
	                    subject: Name(
	                        ExprName {
	                            range: 6..9,
	                            id: "foo",
	                            ctx: Load,
	                        },
	                    ),
	                    cases: [
	                        MatchCase {
	                            range: 15..51,
	                            pattern: MatchClass(
	                                PatternMatchClass {
	                                    range: 20..37,
	                                    cls: Name(
	                                        ExprName {
	                                            range: 20..23,
	                                            id: "Foo",
	                                            ctx: Load,
	                                        },
	                                    ),
	                                    arguments: PatternArguments {
	                                        range: 24..37,
	                                        patterns: [
	                                            MatchAs(
	                                                PatternMatchAs {
	                                                    range: 33..36,
	                                                    pattern: None,
	                                                    name: Some(
	                                                        Identifier {
	                                                            id: "baz",
	                                                            range: 33..36,
	                                                        },
	                                                    ),
	                                                },
	                                            ),
	                                        ],
	                                        keywords: [
	                                            PatternKeyword {
	                                                range: 24..31,
	                                                attr: Identifier {
	                                                    id: "",
	                                                    range: 31..31,
	                                                },
	                                                pattern: MatchValue(
	                                                    PatternMatchValue {
	                                                        range: 30..31,
	                                                        value: NumberLiteral(
	                                                            ExprNumberLiteral {
	                                                                range: 30..31,
	                                                                value: Int(
	                                                                    1,
	                                                                ),
	                                                            },
	                                                        ),
	                                                    },
	                                                ),
	                                            },
	                                        ],
	                                    },
	                                },
	                            ),
	                            guard: None,
	                            body: [
	                                Pass(
	                                    StmtPass {
	                                        range: 47..51,
	                                    },
	                                ),
	                            ],
	                        },
	                    ],
	                },
	            ),
	        ],
	    },
	)
	```
	
	</p>
	</details> 

2. Parsing a list of parameters. Here, our list parsing method makes sure to only call the parse element function when it's a valid list element. A parameter can start either with a `Star`, `DoubleStar`, or `Name` token which corresponds to the 3 `if` conditions. Thus, the `else` block is not required as the list parsing will recover without it.



---

_Label `parser` added by @dhruvmanila on 2024-03-08 06:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-08 06:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:576 on 2024-03-08 08:15_

What I understand from this comment is that without this `parse_match_pattern` call it's possible that the list parsing gets stuck because the `item` parsing function never progress. Is this what it means? Or does it mean that we should always try to parse the patch pattern value because we're after the `=` and we know that we should find a value pattern.

---

_@MichaReiser approved on 2024-03-08 08:15_

---

_@MichaReiser approved on 2024-03-08 08:15_

---

_@dhruvmanila reviewed on 2024-03-08 08:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:576 on 2024-03-08 08:50_

The former. Taking the example from the PR description, the parser being at `1`, but it expects a comma thus it breaks out of parsing the list with an error.

This makes me wonder about the case where there's no pattern after `=` like in the following snippet:
```python
match foo:
	case Foo(bar=, baz):
		pass
```

It adds an error here:

https://github.com/astral-sh/ruff/blob/5e0e1f09793508eef3bfac136df42ee865b55e72/crates/ruff_python_parser/src/parser/pattern.rs#L526-L529

Not sure why it's "Expression expected.", I'll change it to "Expected a pattern" as by this time we've tried all possible pattern parsing.

---

_@MichaReiser reviewed on 2024-03-08 08:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:576 on 2024-03-08 08:56_

Okay, I think we have a different understanding of `stuck`. To me, stuck means that the parser goes into an infinite loop. I dont' think that's the case here because the error recovery bails out if it doesn't parse the `value_pattern`. However, it doesn't recover nicely from the error.

However, there's a fundamental problem with this parsing method now that we always call `parse_match_pattern`... we drop the `value_pattern` in case it isn't a `PatternMatchAs`. I don't think we should be doing this. Can we instead convert `pattern` into ` PatternMatchAs` if we've found a `=`?  

---

_@dhruvmanila reviewed on 2024-03-08 17:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:576 on 2024-03-08 17:51_

From Discord:

> So, the `PatternKeyword` node structure (for keyword patterns like `case Foo(a=1): ...`) is that it contains an identifier and a pattern. We can use an empty `Expr::Name` pattern here and retain the parsed value pattern. This would be similar to the empty `Identifier` patten.

---

_Merged by @dhruvmanila on 2024-03-08 17:58_

---

_Closed by @dhruvmanila on 2024-03-08 17:58_

---

_Branch deleted on 2024-03-08 17:58_

---
