```yaml
number: 5170
title: Explain dangling comments in the formatter
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: explain_dangling_comments_in_the_formatter
created_at: 2023-06-18T13:39:34Z
updated_at: 2023-06-19T12:24:47Z
url: https://github.com/astral-sh/ruff/pull/5170
synced_at: 2026-01-12T03:43:30Z
```

# Explain dangling comments in the formatter

---

_Pull request opened by @konstin on 2023-06-18 13:39_

This documentation change improves the section on dangling comments in the formatter.

---

_Label `formatter` added by @konstin on 2023-06-18 13:39_

---

_Comment by @github-actions[bot] on 2023-06-18 13:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.8±0.06ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1397.8±4.42µs    11.9 MB/sec    1.00   1399.4±2.76µs    11.9 MB/sec
formatter/numpy/globals.py                 1.01    137.7±0.23µs    21.4 MB/sec    1.00    136.2±0.58µs    21.7 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.11ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.05     14.7±0.23ms     2.8 MB/sec    1.00     14.1±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    372.7±1.46µs     7.9 MB/sec    1.00    363.5±1.35µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.04ms     5.4 MB/sec    1.00      7.4±0.06ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1566.7±6.12µs    10.6 MB/sec    1.00   1525.9±3.18µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    168.5±0.24µs    17.5 MB/sec    1.00    164.5±0.21µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.01ms     7.5 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.2±0.17ms     4.0 MB/sec    1.00      9.8±0.19ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1997.9±55.96µs     8.3 MB/sec    1.00  1988.9±47.77µs     8.4 MB/sec
formatter/numpy/globals.py                 1.01    192.0±6.01µs    15.4 MB/sec    1.00    190.8±7.33µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.09ms     6.3 MB/sec    1.01      4.1±0.21ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     19.0±0.32ms     2.1 MB/sec    1.00     18.8±0.29ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.11ms     3.4 MB/sec    1.02      5.0±0.09ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   576.6±12.40µs     5.1 MB/sec    1.02   585.5±11.30µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.14ms     3.0 MB/sec    1.00      8.3±0.16ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.17ms     4.1 MB/sec    1.00      9.9±0.20ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.03ms     7.9 MB/sec    1.00      2.1±0.04ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    238.3±5.72µs    12.4 MB/sec    1.00    233.7±5.51µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.07ms     5.7 MB/sec    1.00      4.4±0.07ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:107 on 2023-06-18 16:50_

is there a full stop/new sentence missing after "close to them"?

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:118 on 2023-06-18 16:50_

if the comment inside the empty list is an example of a dangling comment, can we make it say something about that instead of talking about the python code?

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:109 on 2023-06-18 16:50_

should that be "where there is _no_ node?"

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:171 on 2023-06-18 16:53_

(mis)attributed: maybe i'm missing something but to me, talking about "attribution" sounds like the work of the parser whereas we are talking about format output, right?

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:187 on 2023-06-18 16:55_

i think it would be helpful with at least some pseudocode/comment explaining how `or_else_comments_start` is calculated/constructed

---

_@davidszotten reviewed on 2023-06-18 16:55_

thanks, this was super helpful! added a few minor comments

---

_Comment by @konstin on 2023-06-18 20:37_

Thanks for the review, updated the explanation

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:107 on 2023-06-19 08:09_

```suggestion
Comments are automatically attached as `Leading` or `Trailing` to a node close to them, or `Dangling`
```

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:161 on 2023-06-19 08:18_

i had to read the section below several times to follow (including why you were talking about the "trailing cond" comment when fixing the "leading else" comment. if i understand correctly it's because those get lumped together into the dangling_comments list. if that's correct could we write that out? though reading yet again i'm not even sure i did understand correctly. 

sorry for the back-and forth on this!

on the other hand, this pr definitely improves on the current state so we could also just land it and iterate further later

---

_@davidszotten reviewed on 2023-06-19 08:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:99 on 2023-06-19 09:15_

Unfortunately, we don't host our rust documentation anywhere. We could otherwise link to the rust documentation.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:109 on 2023-06-19 09:17_

```suggestion
needs to be overridden in [`place_comment`](https://github.com/astral-sh/ruff/blob/be11cae619d5a24adb4da34e64d3c5f270f9727b/crates/ruff_python_formatter/src/comments/placement.rs#L13) in `placement.rs`, which this section is about.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:176 on 2023-06-19 09:19_

```suggestion
[`handle_in_between_bodies_own_line_comment`](https://github.com/astral-sh/ruff/blob/be11cae619d5a24adb4da34e64d3c5f270f9727b/crates/ruff_python_formatter/src/comments/placement.rs#L196) and marking them as dangling. Similarly, we find and
```

---

_@MichaReiser approved on 2023-06-19 09:20_

Thanks for writting this up!

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:99 on 2023-06-19 09:41_

Would be nice to put this somewhere under https://beta.ruff.rs/docs/, the rustdoc is static files only

---

_@konstin reviewed on 2023-06-19 09:41_

---

_@konstin reviewed on 2023-06-19 09:58_

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:161 on 2023-06-19 09:58_

thanks for the feedback, i made the problem of the dangling list throwing different kinds of dangling comments together more explicit

---

_@davidszotten reviewed on 2023-06-19 12:03_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/README.md`:186 on 2023-06-19 12:03_

ah, i see. thanks!

(though reading this makes me wonder if we want to consider adding a way to provide this extra information at the time we move the comments around (in placement.rs) since we've already done the work and do have the context there)

---

_@davidszotten reviewed on 2023-06-19 12:04_

not allowed to approve by gh but 👍 from me

---

_Merged by @konstin on 2023-06-19 12:24_

---

_Closed by @konstin on 2023-06-19 12:24_

---

_Branch deleted on 2023-06-19 12:24_

---
