```yaml
number: 5184
title: Add BestFittingMode
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: best-fitting-mode
created_at: 2023-06-19T15:52:42Z
updated_at: 2023-06-20T16:16:04Z
url: https://github.com/astral-sh/ruff/pull/5184
synced_at: 2026-01-12T03:43:30Z
```

# Add BestFittingMode

---

_Pull request opened by @MichaReiser on 2023-06-19 15:52_

## Summary
Black supports for layouts when it comes to breaking binary expressions:

```rust
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
enum BinaryLayout {
    /// Put each operand on their own line if either side expands
    Default,

    /// Try to expand the left to make it fit. Add parentheses if the left or right don't fit.
    ///
    ///```python
    /// [
    ///     a,
    ///     b
    /// ] & c
    ///```
    ExpandLeft,

    /// Try to expand the right to make it fix. Add parentheses if the left or right don't fit.
    ///
    /// ```python
    /// a & [
    ///     b,
    ///     c
    /// ]
    /// ```
    ExpandRight,

    /// Both the left and right side can be expanded. Try in the following order:
    /// * expand the right side
    /// * expand the left side
    /// * expand both sides
    ///
    /// to make the expression fit
    ///
    /// ```python
    /// [
    ///     a,
    ///     b
    /// ] & [
    ///     c,
    ///     d
    /// ]
    /// ```
    ExpandRightThenLeft,
}
```

Our current implementation only handles `ExpandRight` and `Default` correctly. `ExpandLeft` turns out to be surprisingly hard. This PR adds a new `BestFittingMode` parameter to `BestFitting` to support `ExpandLeft`.

There are 3 variants that `ExpandLeft` must support:

**Variant 1**: Everything fits on the line (easy)

```python
[a, b] + c
```

**Variant 2**: Left breaks, but right fits on the line. Doesn't need parentheses

```python
[
	a,
	b
] + c
```

**Variant 3**: The left breaks, but there's still not enough space for the right hand side. Parenthesize the whole expression:

```python
(
	[
		a, 
		b
	]
	+ ccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc
)
```

Solving Variant 1 and 2 on their own is straightforward The printer gives us this behavior by nesting right inside of the group of left:

```
group(&format_args![
	if_group_breaks(&text("(")),
	soft_block_indent(&group(&format_args![
		left, 
		soft_line_break_or_space(), 
		op, 
		space(), 
		group(&right)
	])),
	if_group_breaks(&text(")"))
])
```

The fundamental problem is that the outer group, which adds the parentheses, always breaks if the left side breaks. That means, we end up with

```python
(
	[
		a,
		b
	] + c
)
```

which is not what we want (we only want parentheses if the right side doesn't fit). 

Okay, so nesting groups don't work because of the outer parentheses. Sequencing groups doesn't work because it results in a right-to-left breaking which is the opposite of what we want. 

Could we use best fitting? Almost! 

```
best_fitting![
	// All flat
	format_args![left, space(), op, space(), right],
	// Break left
	format_args!(group(&left).should_expand(true), space(), op, space(), right],
	// Break all
	format_args![
		text("("), 
		block_indent!(&format_args![
			left, 
			hard_line_break(), 
			op,
			space()
			right
		])
	]
]
```

I hope I managed to write this up correctly. The problem is that the printer never reaches the 3rd variant because the second variant always fits:

* The `group(&left).should_expand(true)` changes the group so that all `soft_line_breaks` are turned into hard line breaks. This is necessary because we want to test if the content fits if we break after the `[`. 
* Now, the whole idea of `best_fitting` is that you can pretend that some content fits on the line when it actually does not. The way this works is that the printer **only** tests if all the content of the variant **up to** the first line break fits on the line (we insert that line break by using `should_expand(true))`. The printer doesn't care whether the rest `a\n, b\n ] + c` all fits on (multiple?) lines. 

Why does breaking right work but not breaking the left? The difference is that we can make the decision whether to parenthesis the expression based on the left expression. We can't do this for breaking left because the decision whether to insert parentheses or not would depend on a lookahead: will the right side break. We simply don't know this yet when printing the parentheses (it would work for the right parentheses but not for the left and indent).

What we kind of want here is to tell the printer: Look, what comes here may or may not fit on a single line but we don't care. Simply test that what comes **after** fits on a line. 

This PR adds a new `BestFittingMode` that has a new `AllLines` option that gives us the desired behavior of testing all content and not just up to the first line break. 

## Test Plan

I added a new example to  `BestFitting::with_mode`

---

_Comment by @MichaReiser on 2023-06-19 15:52_

Current dependencies on/for this PR:
* main
  * **PR #5184** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5184" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #5205** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5205" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #4985** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/4985" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #5207** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5207" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #4986** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/4986" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5184?utm_source=stack-comment).

---

_Review requested from @konstin by @MichaReiser on 2023-06-19 15:53_

---

_Label `formatter` added by @MichaReiser on 2023-06-19 15:53_

---

_Renamed from "Add BeestFittingMode" to "Add BestFittingMode" by @MichaReiser on 2023-06-19 15:56_

---

_Comment by @github-actions[bot] on 2023-06-19 16:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.01ms     5.3 MB/sec    1.00      7.6±0.01ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1586.0±1.64µs    10.5 MB/sec    1.00   1587.6±4.07µs    10.5 MB/sec
formatter/numpy/globals.py                 1.01    152.9±0.28µs    19.3 MB/sec    1.00    152.0±1.21µs    19.4 MB/sec
formatter/pydantic/types.py                1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.07ms     2.6 MB/sec    1.04     16.5±0.06ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.2 MB/sec    1.02      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    513.9±0.96µs     5.7 MB/sec    1.00    507.5±1.10µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.04ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.1 MB/sec    1.07      8.5±0.37ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1747.5±6.62µs     9.5 MB/sec    1.04   1814.8±5.56µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.8±0.47µs    14.8 MB/sec    1.02    204.7±6.16µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.03      3.8±0.01ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.12ms     5.3 MB/sec    1.05      8.1±0.07ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1570.8±29.13µs    10.6 MB/sec    1.04  1634.8±31.98µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    155.0±3.34µs    19.0 MB/sec    1.02    157.8±5.36µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.05ms     8.2 MB/sec    1.06      3.3±0.04ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.01     16.3±0.25ms     2.5 MB/sec    1.00     16.2±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.01      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.7±6.96µs     5.9 MB/sec    1.00   501.6±14.14µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.7 MB/sec    1.01      7.0±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.07      8.6±0.08ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1746.5±34.44µs     9.5 MB/sec    1.02  1789.8±25.17µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.4±4.58µs    14.6 MB/sec    1.00    202.0±5.22µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.04      3.8±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @MichaReiser on 2023-06-19 16:06_

---

_@konstin approved on 2023-06-20 15:58_

---

_Comment by @MichaReiser on 2023-06-20 16:15_

@MichaReiser started a stack merge that includes this pull request via [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5184).

---

_Merged by @MichaReiser on 2023-06-20 16:16_

---

_Closed by @MichaReiser on 2023-06-20 16:16_

---

_Branch deleted on 2023-06-20 16:16_

---

_Comment by @MichaReiser on 2023-06-20 16:16_

@MichaReiser merged this pull request with [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5184).

---
