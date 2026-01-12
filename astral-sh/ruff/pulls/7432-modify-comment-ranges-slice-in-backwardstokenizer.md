```yaml
number: 7432
title: "Modify `comment_ranges` slice in `BackwardsTokenizer`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2023-09-16T03:38:53Z
updated_at: 2023-09-18T08:33:12Z
url: https://github.com/astral-sh/ruff/pull/7432
synced_at: 2026-01-12T02:39:10Z
```

# Modify `comment_ranges` slice in `BackwardsTokenizer`

---

_Pull request opened by @charliermarsh on 2023-09-16 03:38_

## Summary

I was kinda curious to understand this issue (https://github.com/astral-sh/ruff/issues/7426) and just ended up attempting to address it.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-16 03:38_

---

_Review requested from @konstin by @charliermarsh on 2023-09-16 03:38_

---

_Label `internal` added by @charliermarsh on 2023-09-16 03:39_

---

_Comment by @github-actions[bot] on 2023-09-16 03:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:760 on 2023-09-16 14:52_

I must have missed this in the previous PR but it seems that the lexer now always initializes with `after_newline`. The lexer used to have a special `up_to` method indicating that it is guaranteed not to be after a newline to avoid testing for comments. 

I'm bringing this up here because the downside is that we now always perform a binary search to find the last comment, even if the caller can guarantee that the start position isn't after a newline. Bringing back this optimization is probably worth its own PR

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:785 on 2023-09-16 14:53_

Nit: May be worth testing if the `after_newline` boolean still is an optimization, considering that testing whether we're inside of a comment is now a single operation (unlike before where it was necessary to lex-out the full line, worst case, the entire file up to this position)

---

_@MichaReiser approved on 2023-09-16 14:54_

---

_Comment by @codspeed-hq[bot] on 2023-09-16 18:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/slice)

### Merging #7432 will **improve performances by 3%**

<sub>Comparing <code>charlie/slice</code> (ce61771) with <code>main</code> (aae02cf)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/slice` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35.1 ms | 34 ms | +3% |


---

_Merged by @charliermarsh on 2023-09-16 18:04_

---

_Closed by @charliermarsh on 2023-09-16 18:04_

---

_Branch deleted on 2023-09-16 18:04_

---

_@konstin reviewed on 2023-09-18 08:32_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:760 on 2023-09-18 08:32_

i didn't implement this initially because i thought about doing the binary search esp for large files with many comments is expensive to do for every is-expression-parenthesized check, but it doesn't seem to show in our benchmarks. Removing this is also kinda ugly because we have to bring back `after_newline` 

---

_Comment by @konstin on 2023-09-18 08:33_

thanks!

---
