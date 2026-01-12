```yaml
number: 6676
title: "Format `PatternMatchSequence`"
type: pull_request
state: merged
author: harupy
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-FormatPatternSequence
created_at: 2023-08-18T14:39:20Z
updated_at: 2023-08-23T00:58:14Z
url: https://github.com/astral-sh/ruff/pull/6676
synced_at: 2026-01-12T15:55:22Z
```

# Format `PatternMatchSequence`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Close #6645

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Do I need to add extra tests?

---

_@harupy reviewed on 2023-08-18 14:42_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:26 on 2023-08-18 14:42_

Oh `case (...)` is also `PatternMatchSequence`. I need to fix this.

---

_Comment by @MichaReiser on 2023-08-18 15:00_

> Do I need to add extra tests?

You can add additional tests to `crates/ruff_python_formatter/resources/test/fixtures/ruff`. 

We like to add additional tests for handling comments because they tend to be tricky and black's test suite only has very few cases for it. 

---

_Comment by @github-actions[bot] on 2023-08-18 15:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.03ms    10.2 MB/sec    1.00      4.0±0.02ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    844.8±3.91µs    19.7 MB/sec    1.01    851.4±8.66µs    19.6 MB/sec
formatter/numpy/globals.py                 1.00     90.8±0.63µs    32.5 MB/sec    1.00     91.2±0.51µs    32.4 MB/sec
formatter/pydantic/types.py                1.00   1651.5±9.16µs    15.4 MB/sec    1.00  1657.8±26.39µs    15.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.0±0.04ms     3.4 MB/sec    1.00     12.1±0.05ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.03    457.4±2.07µs     6.5 MB/sec    1.00    444.9±8.05µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.23ms     4.0 MB/sec    1.00      6.3±0.08ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.03      6.5±0.02ms     6.3 MB/sec    1.00      6.3±0.09ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1443.0±4.78µs    11.5 MB/sec    1.00  1429.7±12.72µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±0.43µs    17.9 MB/sec    1.00    165.9±0.47µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.07ms    11.0 MB/sec    1.00      3.7±0.06ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   763.0±11.59µs    21.8 MB/sec    1.00   759.0±10.86µs    21.9 MB/sec
formatter/numpy/globals.py                 1.00     79.0±2.04µs    37.3 MB/sec    1.01     79.7±1.39µs    37.0 MB/sec
formatter/pydantic/types.py                1.00  1528.3±27.20µs    16.7 MB/sec    1.01  1542.0±53.78µs    16.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.18ms     3.3 MB/sec    1.02     12.7±0.44ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.02      3.5±0.07ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.8±4.83µs     6.8 MB/sec    1.01    435.6±5.82µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.02      6.6±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.10ms     5.8 MB/sec    1.02      7.2±0.13ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1490.7±24.44µs    11.2 MB/sec    1.02  1526.4±29.92µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±2.52µs    17.5 MB/sec    1.03    173.4±3.42µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.1 MB/sec    1.03      3.2±0.06ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @harupy on 2023-08-19 04:09_

@MichaReiser Got it, I will add tests

---

_@harupy reviewed on 2023-08-19 06:53_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:151 on 2023-08-19 06:53_

Is there a way to detect whether the sequence is a list or a tuple? It looks like `PatternMatchSequence` only knows its range and elements in the sequence and doesn't know what the sequence type is.

---

_@MichaReiser reviewed on 2023-08-21 07:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:151 on 2023-08-21 07:17_

As it seems, not by looking at the AST. But you can get this information when inspecting the source:

```rust
let parentheses = match &f.context().source()[item.range()] {
	['(', ...] => Some(('(', ')')),
	['[', ...] => Some(('[', ']')),
	_ => None
```

---

_Label `formatter` added by @konstin on 2023-08-21 08:32_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:193 on 2023-08-21 12:55_

can you add `(["a", "b"])` as a test case?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:67 on 2023-08-21 12:59_

Does this also work?
```suggestion
                f.join_with(&format_args![text(","), space()])
                    .entries(patterns.iter().map(AsFormat::format))
                    .finish()?
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:63 on 2023-08-21 12:59_

Please add a comment why you're not using `join_comma_separated` here

---

_@konstin approved on 2023-08-21 13:00_

---

_@harupy reviewed on 2023-08-21 13:18_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:67 on 2023-08-21 13:18_

worked!

---

_@harupy reviewed on 2023-08-21 13:18_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:193 on 2023-08-21 13:18_

Added!

---

_@harupy reviewed on 2023-08-21 13:44_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:69 on 2023-08-21 13:44_

This is incorrect. I will fix it.

---

_@harupy reviewed on 2023-08-21 13:52_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 13:52_

With `join_comma_separated`, this is formatted to:

```python
    case "NOT_YET_IMPLEMENTED_PatternMatchValue",
    "NOT_YET_IMPLEMENTED_PatternMatchValue",:
```

which is a syntax error.

---

_@konstin reviewed on 2023-08-21 13:53_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:69 on 2023-08-21 13:53_

i'm fine if this is a single line comment like "We can't also use `join_comma_separated` since we don't use a trailing a comma without parentheses"

---

_@harupy reviewed on 2023-08-21 13:56_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:69 on 2023-08-21 13:56_

A trailing comma is not a syntax error, but we can't wrap the line like https://github.com/astral-sh/ruff/pull/6676#discussion_r1300145091.

---

_@harupy reviewed on 2023-08-21 13:57_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 13:57_

Should this be formatted like this?

```python
    case "NOT_YET_IMPLEMENTED_PatternMatchValue", "NOT_YET_IMPLEMENTED_PatternMatchValue":
```

---

_@harupy reviewed on 2023-08-21 13:58_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 13:58_

or 

```python
    case (
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
    ):
        pass
```

?

---

_@harupy reviewed on 2023-08-21 14:08_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:45 on 2023-08-21 14:08_

This is still not correct because `parenthesized` always wraps the sequence with `()`.

---

_@konstin reviewed on 2023-08-21 14:46_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 14:46_

not sure tbh. black does not add parentheses even if the line is too long, but i also think it would be good if we would break instead of writing an overlong lines. Also black's behavior is inconsistent with other overlong tuples, e.g. 
```python
match x:
    case "NOT_YET_IMPLEMENTED_PatternMatchValue", "NOT_YET_IMPLEMENTED_PatternMatchValue", "aslödjhkfö":
        pass

b = "NOT_YET_IMPLEMENTED_PatternMatchValue", "NOT_YET_IMPLEMENTED_PatternMatchValue", "aslödjhkfö"
```
becomes
```python
match x:
    case "NOT_YET_IMPLEMENTED_PatternMatchValue", "NOT_YET_IMPLEMENTED_PatternMatchValue", "aslödjhkfö":
        pass

b = (
    "NOT_YET_IMPLEMENTED_PatternMatchValue",
    "NOT_YET_IMPLEMENTED_PatternMatchValue",
    "aslödjhkfö",
)
```

CC @MichaReiser what do you think?

---

_@MichaReiser reviewed on 2023-08-21 15:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 15:07_

I'm not well at the moment and this is taking more brain capacity than I have right now. I hope to get back to this tomorrow

---

_@harupy reviewed on 2023-08-21 23:08_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-21 23:08_

> i also think it would be good if we would break instead of writing an overlong lines.

+1

---

_@MichaReiser reviewed on 2023-08-22 06:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-22 06:28_

The sequence pattern is a bit strange IMO because matching `[a, b]` or `(a, b)` or `a, b` are semantically identical. Meaning, the sequence is neither a list nor a tuple... it's its own thing. 

But I agree, I would expect the sequences to break the same as tuples (and lists) do

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:194 on 2023-08-22 07:39_

https://github.com/psf/black/issues/3726 and https://github.com/psf/black/issues/3403 seem related.

---

_@harupy reviewed on 2023-08-22 07:39_

---

_Review requested from @MichaReiser by @harupy on 2023-08-22 09:31_

---

_Review requested from @konstin by @harupy on 2023-08-22 09:31_

---

_@konstin approved on 2023-08-22 09:48_

---

_Comment by @charliermarsh on 2023-08-23 00:36_

Did a quick read -- gonna merge to keep things moving along. Can always address follow-ups in new PRs.

---

_Merged by @charliermarsh on 2023-08-23 00:44_

---

_Closed by @charliermarsh on 2023-08-23 00:44_

---

_Branch deleted on 2023-08-23 00:57_

---
