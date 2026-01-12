```yaml
number: 6800
title: "Handle pattern parentheses in `FormatPattern`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/paren-value
created_at: 2023-08-23T01:58:17Z
updated_at: 2023-08-25T04:18:19Z
url: https://github.com/astral-sh/ruff/pull/6800
synced_at: 2026-01-12T15:55:22Z
```

# Handle pattern parentheses in `FormatPattern`

---

_@charliermarsh_

## Summary

This PR fixes the duplicate-parenthesis problem that's visible in the tests from https://github.com/astral-sh/ruff/pull/6799. The issue is that we might have parentheses around the entire match-case pattern, like in `(1)` here:

```python
match foo:
    case (1):
        y = 0
```

In this case, the inner expression (`1`) will _think_ it's parenthesized, but we'll _also_ detect the parentheses at the case level -- so they get rendered by the case, then again by the expression. Instead, if we detect parentheses at the case level, we can force-off the parentheses for the pattern using a design similar to the way we handle parentheses on expressions.

Closes https://github.com/astral-sh/ruff/issues/6753.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-08-23 02:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:581 on 2023-08-23 02:00_

Ah, hmm... I guess this is wrong. The input text is `case ((1)):`, but we're stripping the _inner_ parentheses.

---

_Label `formatter` added by @charliermarsh on 2023-08-23 02:00_

---

_@charliermarsh reviewed on 2023-08-23 02:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:581 on 2023-08-23 02:02_

Or, maybe this is acceptable? We seem to drop these kinds of parentheses in _some_ contexts: https://play.ruff.rs/bc8cf10e-e3ae-43e2-a6f7-6f7f2ad08b09

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 02:02_

---

_@charliermarsh reviewed on 2023-08-23 02:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:689 on 2023-08-23 02:04_

This is wrong, it should be on the `[`.

---

_@charliermarsh reviewed on 2023-08-23 02:16_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:689 on 2023-08-23 02:16_

But I'll fix it in a separate PR, since it's specific to the sequence match.

---

_Comment by @github-actions[bot] on 2023-08-23 02:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.7±0.05ms     8.7 MB/sec    1.00      4.7±0.04ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.02    999.6±5.17µs    16.7 MB/sec    1.00   983.7±10.99µs    16.9 MB/sec
formatter/numpy/globals.py                 1.01     95.4±1.12µs    30.9 MB/sec    1.00     94.8±1.33µs    31.1 MB/sec
formatter/pydantic/types.py                1.01  1943.6±18.59µs    13.1 MB/sec    1.00  1915.0±32.64µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.00     11.9±0.17ms     3.4 MB/sec    1.01     12.0±0.14ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.02ms     5.2 MB/sec    1.00      3.2±0.04ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    456.0±6.73µs     6.5 MB/sec    1.00    456.6±4.04µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.06ms     4.0 MB/sec    1.00      6.2±0.08ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.06ms     6.4 MB/sec    1.00      6.4±0.07ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1411.5±13.50µs    11.8 MB/sec    1.04  1464.2±83.98µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.5±2.07µs    17.8 MB/sec    1.00    165.6±2.57µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.03ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      6.4±0.29ms     6.4 MB/sec    1.00      6.2±0.21ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.02  1283.4±57.25µs    13.0 MB/sec    1.00  1257.8±48.67µs    13.2 MB/sec
formatter/numpy/globals.py                 1.00    115.2±5.20µs    25.6 MB/sec    1.01    116.5±8.42µs    25.3 MB/sec
formatter/pydantic/types.py                1.01      2.5±0.11ms    10.1 MB/sec    1.00      2.5±0.09ms    10.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.9±0.76ms     2.3 MB/sec    1.03     18.5±0.61ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.16ms     3.4 MB/sec    1.02      4.9±0.18ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.03   615.4±44.17µs     4.8 MB/sec    1.00   599.2±22.48µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.38ms     2.8 MB/sec    1.01      9.2±0.36ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.27ms     4.1 MB/sec    1.00      9.9±0.31ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.10ms     8.0 MB/sec    1.00      2.1±0.07ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   266.6±22.51µs    11.1 MB/sec    1.00   257.9±10.99µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.15ms     5.7 MB/sec    1.00      4.4±0.10ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila reviewed on 2023-08-23 04:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:581 on 2023-08-23 04:19_

Yeah, I think this should be good. The additional parentheses are unnecessary although this deviates from Black which keeps the parentheses.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_310__pattern_matching_simple.py.snap`:120 on 2023-08-23 04:48_

Regarding the addition of parentheses around unions, I believe that will be handled in `MatchOr`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_310__pattern_matching_extras.py.snap`:373 on 2023-08-23 04:49_

Unrelated but shouldn't we consider the magic trailing comma and keep each pattern on a separate line?

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/match_case.rs`:46 on 2023-08-23 04:53_

Do we actually need to preserve the parentheses? Black does but in other nodes we don't:
```python
while (True):
	pass
```

Gets formatted to:
```python
while True:
	pass
```

---

_@dhruvmanila reviewed on 2023-08-23 04:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/match_case.rs`:46 on 2023-08-23 06:38_

Our rule for expressions are:

* Top level expression (inside a statement): Remove unnecessary parentheses, see your `while (True)` example
* nested expressions: Preserve the parentheses (except for `await`  where Black removes parentheses). 

IMO the behavior should be the same for `Patterns`. Remove unnecessary parentheses around the outermost pattern but preserve them for nested patterns. See https://github.com/astral-sh/ruff/issues/6753

Meaning, we should use `parenthesize_if_expands` here instead of preserving the parentheses.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/mod.rs`:78 on 2023-08-23 06:40_

For consistency 
```suggestion
            format_pattern.fmt(f)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/mod.rs`:48 on 2023-08-23 06:41_

Nit
```suggestion
        let format_pattern = format_with(|f| match pattern {
            Pattern::MatchValue(pattern) => pattern.format().fmt(f),
            Pattern::MatchSingleton(pattern) => pattern.format().fmt(f),
            Pattern::MatchSequence(pattern) => pattern.format().fmt(f),
            Pattern::MatchMapping(pattern) => pattern.format().fmt(f),
            Pattern::MatchClass(pattern) => pattern.format().fmt(f),
            Pattern::MatchStar(pattern) => pattern.format().fmt(f),
            Pattern::MatchAs(pattern) => pattern.format().fmt(f),
            Pattern::MatchOr(pattern) => pattern.format().fmt(f),
        });

        let parenthesize = match self.parentheses {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_310__pattern_matching_extras.py.snap`:373 on 2023-08-23 06:42_

We probably should

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:581 on 2023-08-23 06:43_

I think this is fine (see my comment above). But only for the `case` level pattern, not for nested patterns.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 06:45_

Can you explain why calling `pattern.format` isn't sufficient? Why can the pattern and the `MatchCase` both have trailing opening parentheses comments.

---

_@MichaReiser reviewed on 2023-08-23 06:46_

Does this address https://github.com/astral-sh/ruff/issues/6753 ? If not, what's missing? Could we implement the required changes to match the expected output

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:46 on 2023-08-23 13:24_

I'll try out `parenthesize_if_expands`.

---

_@charliermarsh reviewed on 2023-08-23 13:24_

---

_@charliermarsh reviewed on 2023-08-23 13:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 13:26_

Consider:

```python
match thing:
    case ( # outer
        [  # inner
            1
        ]
    )
```

The `# outer` is attached to the `MatchCase`, but the `# inner` is part of the pattern itself. (Not all patterns have this behavior.)

---

_@MichaReiser reviewed on 2023-08-23 15:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 15:28_

Hmm, so it depends on whether the pattern has its own parentheses? How does this work if you have nested pattern, which doesn't go through the match case formatting?

```
match thing:
	case [
		( # outer
			[ # inner
				1, 
			]
		)
	]: ... 

---

_@charliermarsh reviewed on 2023-08-23 15:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:46 on 2023-08-23 15:43_

I think this also requires knowing whether the pattern has its own parentheses (e.g., for sequence patterns).

---

_Comment by @charliermarsh on 2023-08-23 15:43_

> Does this address https://github.com/astral-sh/ruff/issues/6753 ? If not, what's missing?

Yes I believe it should (sorry, I missed that issue, I stumbled upon this independently). Added a test + linked the issue.

---

_Comment by @charliermarsh on 2023-08-23 15:44_

I still need to figure out the `parenthesize_if_expands`, working on that now.

---

_@charliermarsh reviewed on 2023-08-23 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 15:59_

Both of these cases work as expected as of the following branch (#6801):

```python
match thing:
    case [
		( # outer
			[ # inner
				1,
			]
		)
	]: ...
    case [ # outer
		( # inner outer
			[ # inner
				1,
			]
		)
	]: ...
```

`# inner outer` gets assigned as a leading end-of-line comment via our standard parenthesized comment handling, which then gets rendered as an open parenthesis comment.


---

_@charliermarsh reviewed on 2023-08-23 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 15:59_

Actually, let me confirm that that's _why_ it works, hang on.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:00_

Okay, does that mean we don't need the handling in `MatchCase`?

---

_@MichaReiser reviewed on 2023-08-23 16:00_

---

_@charliermarsh reviewed on 2023-08-23 16:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:01_

Err, I think it works because it's a leading end-of-line comment which we always treat as an open parenthesis comment. (And the comments on the inner square brackets work because they're correctly marked as dangling for the sequence pattern type.)

---

_@charliermarsh reviewed on 2023-08-23 16:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:03_

I guess we kind of get this wrong:

```python
    case [ # outer
        # own line
		( # inner outer
			[ # inner
				1,
			]
		)
	]:
        pass
```

It renders as:

```python
    case [  # outer
        (
            # own line
            # inner outer
            [  # inner
                1,
            ]
        )
    ]:
```

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:08_

We might be able to remove this, I'm not sure yet, I'll explore it.

---

_@charliermarsh reviewed on 2023-08-23 16:08_

---

_@charliermarsh reviewed on 2023-08-23 16:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:12_

(I'd thought we wanted to preserve the parentheses here always which complicated this a bit.)

---

_@charliermarsh reviewed on 2023-08-23 16:40_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:41 on 2023-08-23 16:40_

Ok, I was able to remove the dangling comment from `match_case`.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 16:40_

---

_Comment by @charliermarsh on 2023-08-23 16:41_

Okay @MichaReiser I think this is ready for review based on your feedback.

---

_@charliermarsh reviewed on 2023-08-23 16:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/match_case.rs`:58 on 2023-08-23 16:42_

This is relatively similar to `maybe_parenthesize_expression`...

---

_@MichaReiser approved on 2023-08-24 06:42_

Nice, I love it that we can reuse the `NeedsParentheses` concept

---

_@konstin approved on 2023-08-24 09:57_

---

_Merged by @charliermarsh on 2023-08-25 03:45_

---

_Closed by @charliermarsh on 2023-08-25 03:45_

---

_Branch deleted on 2023-08-25 03:45_

---
