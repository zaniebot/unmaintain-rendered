```yaml
number: 5596
title: "Extend formatter IR to support Black's expression formatting"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: conditional-group
created_at: 2023-07-07T16:28:52Z
updated_at: 2023-07-11T12:33:38Z
url: https://github.com/astral-sh/ruff/pull/5596
synced_at: 2026-01-12T03:36:55Z
```

# Extend formatter IR to support Black's expression formatting

---

_Pull request opened by @MichaReiser on 2023-07-07 16:28_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR makes two extensions to support Black's expression formatting. 

### `ConditionalGroup`

The first extension adds a new `ConditionalGroup` IR element. The semantic is the same as for `Group`, except that it has a condition controlling whether the enclosing content should be grouped or not. Another way to think about this is: Only group this content if some condition is true, but the condition only gets evaluated when printing the IR. 

This is necessary to support Black's formatting for expressions where the enclosing parentheses are optional, for example Black only adds parentheses around the condition of an `if` statement if the expression, after breaking any lists, dicts, etc., does not fit on a line:

```python
# We want 
if a + [ 
	b,
	c
]: 
	pass

# Rather than
if a
	+ [b, c]
: 
	pass
```

which is invalid syntax. Meaning we only want to break after left-parens (`(`, `[`, `{`), but not before operators. 

However, we do want to break **before** operators when the whole expression does not fit:

```python
# We want
if (
	a
	+ [b, c]
): pass

# rather than
if (
	a + [
		b, 
		c
	]
): pass
```

If the whole expression does not fit. You can see, how this is the opposite of when not adding the parentheses. 

The way this extension solves this problem is to *remove* the binary expression groups (gate them with a condition) when the whole expression fits, but *keep* them when the whole expression expands. 

Another way to think about the new IR is that this is the same as the following, but as custom IR to avoid interning `content`.

```
if_group_breaks(group(content), parentheses_group_id)),
if_group_fits_on_line(content, parentheses_group_id)
```

### `FitsExpanded`

The way we implement the adding of the optional parentheses is by wrapping the whole `if` condition by a group. However, this creates a problem if an inner parenthesized expression expands, because this would automatically expand all enclosing groups, including the group that adds the optional parentheses. But we don't want the optional parentheses if a parenthesized expression expands. 

The way this PR solves this problem is by introducing a new `FitsExpanded` IR that, similar to best fitting, acts as an expands boundary. Meaning, it won't expand the enclosing parentheses `group` if any of its content expands (a parenthesized expression). Other than best fitting, it also has a different `fits` definition. The content inside a `FitsExpanded` doesn't get measured in the "all flat" mode, instead, it gets measured assuming all its inner content will expand (what's the least space that is required, rather than what is the most space that it requires). 

`FitsExpanded` also supports an optional `Condition` to control when this changed semantic applies or not. This is necessary because we want to keep the list expression together, if possible, when we break before an operator

```python
# We want
if (
	a
	+ [b, c]
): pass

# Instead of 
if (
	a + [
		b, 
		c
	]
): pass
```

## Test Plan

I added a few doctests and use the new IR in the next IR to format expressions.


### Reference

This behavior mimics Blacks [`can_omit_invisible_parentheses`](https://github.com/psf/black/blob/d1248ca9beaf0ba526d265f4108836d89cf551b7/src/black/lines.py#L881-L963) and [transformer selection](https://github.com/psf/black/blob/d1248ca9beaf0ba526d265f4108836d89cf551b7/src/black/linegen.py#L601-L604). 

Ruff does not test whether a parenthesized expression is at the start or end of line. This is done inside of the printer (expanding after an opening parentheses always has the consequence that the expression now is at the end of the line)

---

_Comment by @MichaReiser on 2023-07-07 16:29_

Current dependencies on/for this PR:
* main
  * **PR #5596** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5596" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #5608** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5608" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #5609** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5609" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #5643** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5643" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #5644** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5644" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #5615** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5615" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5596?utm_source=stack-comment).

---

_Renamed from "Use different formatting logic depending on whether an expression is parenthesized" to "Extend formatter IR to support Black's expression formatting" by @MichaReiser on 2023-07-08 08:07_

---

_Label `formatter` added by @MichaReiser on 2023-07-08 08:07_

---

_Marked ready for review by @MichaReiser on 2023-07-08 09:20_

---

_Review requested from @konstin by @MichaReiser on 2023-07-08 09:23_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-08 09:23_

---

_Comment by @github-actions[bot] on 2023-07-08 09:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1844.7±2.43µs     9.0 MB/sec    1.00   1843.5±5.33µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    205.6±0.59µs    14.4 MB/sec    1.00    205.7±0.36µs    14.3 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.01ms     6.4 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.09ms     3.0 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.6±1.12µs     6.8 MB/sec    1.00    435.5±2.80µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.6±8.16µs    11.3 MB/sec    1.00   1464.1±3.48µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.4±0.26µs    17.7 MB/sec    1.00    166.9±0.51µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.7±0.33ms     3.5 MB/sec    1.01     11.9±0.30ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.21ms     6.2 MB/sec    1.00      2.7±0.10ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   299.9±15.36µs     9.8 MB/sec    1.01   302.4±16.71µs     9.8 MB/sec
formatter/pydantic/types.py                1.01      5.8±0.21ms     4.4 MB/sec    1.00      5.7±0.23ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.0±0.38ms     2.0 MB/sec    1.00     19.9±0.37ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.14ms     3.2 MB/sec    1.00      5.1±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   635.8±27.72µs     4.6 MB/sec    1.00   635.3±25.22µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.24ms     2.9 MB/sec    1.02      9.1±0.37ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.30ms     4.0 MB/sec    1.00     10.1±0.23ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.8 MB/sec    1.02      2.2±0.15ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    250.1±9.72µs    11.8 MB/sec    1.01    253.2±9.66µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.16ms     5.6 MB/sec    1.00      4.5±0.15ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_formatter/src/builders.rs`:1416 on 2023-07-11 09:27_

nit: the `&` on the `&str` seems redundant

---

_Review comment by @konstin on `crates/ruff_formatter/src/builders.rs`:1434 on 2023-07-11 09:28_

```suggestion
/// let content = format_with(|f| {
```

---

_Review comment by @konstin on `crates/ruff_formatter/src/builders.rs`:1476 on 2023-07-11 09:53_

these examples are great

---

_Review comment by @konstin on `crates/ruff_formatter/src/builders.rs`:2010 on 2023-07-11 09:56_

do we need a custom limit here if we already got the line break after the comment?

---

_@konstin approved on 2023-07-11 10:23_

---

_@MichaReiser reviewed on 2023-07-11 11:08_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:1416 on 2023-07-11 11:08_

Unfortunately not. I think it is because `field` expects a `&Debug`. `&str` does implement `Debug` but it needs the `&` to get the reference to the debug implementation.

```
error[E0277]: the size for values of type `str` cannot be known at compilation time
    --> crates/ruff_formatter/src/builders.rs:1416:31
     |
1416 |             .field("content", "{{content}}")
     |                               ^^^^^^^^^^^^^ doesn't have a size known at compile-time
     |
     = help: the trait `Sized` is not implemented for `str`
     = note: required for the cast from `&'static str` to `&dyn Debug`
help: consider borrowing the value, since `&&'static str` can be coerced into `&dyn Debug`
     |
1416 |             .field("content", &"{{content}}")
     |                               +

```

---

_@MichaReiser reviewed on 2023-07-11 11:11_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:2010 on 2023-07-11 11:11_

You're right. I can remove it because of the `expand_parent`. 

---

_Merged by @MichaReiser on 2023-07-11 11:20_

---

_Closed by @MichaReiser on 2023-07-11 11:20_

---

_Branch deleted on 2023-07-11 11:20_

---
