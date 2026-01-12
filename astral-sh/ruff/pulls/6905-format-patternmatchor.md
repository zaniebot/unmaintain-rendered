```yaml
number: 6905
title: "Format `PatternMatchOr`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-pattern-match-or
created_at: 2023-08-26T21:45:11Z
updated_at: 2023-12-01T23:46:20Z
url: https://github.com/astral-sh/ruff/pull/6905
synced_at: 2026-01-12T15:55:22Z
```

# Format `PatternMatchOr`

---

_@LaBatata101_

## Summary
There was only one case where I couldn't figure it out how to format properly, which is the following
```python
match x:
    case (
        (a)
        | # trailing
        ( b )
    ):
        ...
```
That code is being formatted to this
```python
match x:
    case (
        (
            a  # trailing
        )
        | (b)
    ):
        ...
```
But it should be formatted to this
```python
match x:
    case (
        (a)  # trailing
        | (b)
    ):
        ...
```

Closes #6643

## Test Plan

`cargo test`


---

_Comment by @LaBatata101 on 2023-08-26 21:50_

Now clippy complains about `not_yet_implemented_custom_text` never being used. Should this function be removed?

And look at that amount of lines removed ☺️ 

---

_Comment by @codspeed-hq[bot] on 2023-08-26 21:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/LaBatata101:format-pattern-match-or)

### Merging #6905 will **not alter performance**

<sub>Comparing <code>LaBatata101:format-pattern-match-or</code> (25e5224) with <code>main</code> (eae59cf)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @MichaReiser on 2023-08-27 08:57_

> Now clippy complains about `not_yet_implemented_custom_text` never being used. Should this function be removed?
> 
> And look at that amount of lines removed ☺️

Finally, our last syntax is now implemented. Yes, we can remove that function.

---

_Label `formatter` added by @MichaReiser on 2023-08-27 08:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:181 on 2023-08-28 07:31_

Nice!

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_or.rs`:31 on 2023-08-28 07:52_

Using the `in_parentheses_only` builder here seems correct but isn't sufficient without using `optional_parentheses` in the match case formatting. I created an issue to track that work #6933

---

_@MichaReiser approved on 2023-08-28 07:52_

Neat! Thanks for working on the last pattern.

---

_Comment by @MichaReiser on 2023-08-28 07:58_

This PR improves the similarity index for CPython from cpython     0.76076 to 0.76081

---

_Merged by @MichaReiser on 2023-08-28 08:09_

---

_Closed by @MichaReiser on 2023-08-28 08:09_

---

_Branch deleted on 2023-12-01 23:46_

---
