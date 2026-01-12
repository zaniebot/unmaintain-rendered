```yaml
number: 6380
title: Avoid hard line break after dangling open-parenthesis comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/trailing-comment
created_at: 2023-08-07T01:50:18Z
updated_at: 2023-08-07T14:28:32Z
url: https://github.com/astral-sh/ruff/pull/6380
synced_at: 2026-01-12T02:52:04Z
```

# Avoid hard line break after dangling open-parenthesis comments

---

_Pull request opened by @charliermarsh on 2023-08-07 01:50_

## Summary

Given:

```python
[  # comment
    first,
    second,
    third
]  # another comment
```

We were adding a hard line break as part of the formatting of `# comment`, which led to the following formatting:

```python
[first, second, third]  # comment
  # another comment
```

Closes https://github.com/astral-sh/ruff/issues/6367.


---

_@charliermarsh reviewed on 2023-08-07 01:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:279 on 2023-08-07 01:50_

A dedicated formatter for these kinds of open-parenthetical methods, rather than using the generic `dangling_comments` formatter which always hard-breaks after comments.

---

_Renamed from "Avoid hard line break after dangling inner-parenthesis comments" to "Avoid hard line break after dangling open-parenthesis comments" by @charliermarsh on 2023-08-07 02:20_

---

_Review requested from @konstin by @charliermarsh on 2023-08-07 02:46_

---

_Comment by @github-actions[bot] on 2023-08-07 03:19_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.52ms     4.4 MB/sec    1.02      9.5±0.58ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1779.9±95.39µs     9.4 MB/sec    1.05  1874.2±107.38µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01   229.4±25.79µs    12.9 MB/sec    1.00   227.0±15.22µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.25ms     6.6 MB/sec    1.03      4.0±0.26ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.04     13.0±0.79ms     3.1 MB/sec    1.00     12.5±0.75ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.3±0.29ms     5.0 MB/sec    1.00      3.3±0.15ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   482.9±33.07µs     6.1 MB/sec    1.01   486.7±32.76µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.32ms     4.3 MB/sec    1.06      6.3±0.34ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.31ms     6.2 MB/sec    1.04      6.9±0.32ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1441.5±80.11µs    11.6 MB/sec    1.01  1449.6±81.72µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   169.9±12.15µs    17.4 MB/sec    1.00   163.5±11.27µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.9±0.14ms     8.8 MB/sec    1.00      2.9±0.16ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.05ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1518.7±9.23µs    11.0 MB/sec    1.00  1519.8±31.79µs    11.0 MB/sec
formatter/numpy/globals.py                 1.00    153.3±1.63µs    19.3 MB/sec    1.01    154.8±4.75µs    19.1 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.03ms     7.6 MB/sec    1.00      3.3±0.03ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.10ms     4.0 MB/sec    1.00     10.3±0.07ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.02ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    294.8±2.24µs    10.0 MB/sec    1.01    296.5±3.25µs    10.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.04ms     5.5 MB/sec    1.00      4.6±0.03ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.04ms     7.2 MB/sec    1.00      5.6±0.05ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1104.3±10.16µs    15.1 MB/sec    1.00   1105.6±9.20µs    15.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    115.1±1.19µs    25.6 MB/sec    1.00    115.5±1.13µs    25.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.03ms    10.5 MB/sec    1.01      2.4±0.02ms    10.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:230 on 2023-08-07 07:23_

Nit: I find `parenthetical` difficult to understand. I think we just used the term `parenthesized` for terms enclosed by any parentheses.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:158 on 2023-08-07 07:27_

Nit: Should we move the `line_suffix` into `parenthetical_comments` to make it consistent with other comment formatting methods?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:238 on 2023-08-07 07:27_

Nit: How about `trailing_open_parentheses_comments(` 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:250 on 2023-08-07 07:28_

It would make sense to add a debug assertion that `comment` is a `end_of_line` comment because this implementation doesn't handle own line comments correctly.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:257 on 2023-08-07 07:30_

Is it possible for a `(` to have more than one dangling comment? 

Either way, this results in `(  # first#second` if a `(` happens to have more than one comment which isn't ideal. I think we should just remove the `first` check, similar to how it is done in `trailing_comments`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:156 on 2023-08-07 07:32_

Do we need this outer group anymore? There's nothing anymore that expands the parent (dangling comments always rendered a hard line break, forcing the group to expand). Or should `parenthetical_comments` render an `expand_parent`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 07:33_

IMO: The old formatting expressed the intent of the user better.

We, so far, have taken the stand that trailing comments should remain as closely as possible to what they originally commented (see [internal notion](https://www.notion.so/astral-sh/Black-Deviations-65977bec23b64aee9d28cc1787badfdb?pvs=4#b346e0ba9cd84f8e93ed0207e5912048)). Meaning they should, by default, expand their parent. I think this should also help that ignore comments move to far away, changing what they suppress.

---

_@MichaReiser approved on 2023-08-07 07:35_

---

_Label `formatter` added by @MichaReiser on 2023-08-07 07:37_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:238 on 2023-08-07 07:43_

maybe `dangling_open_parentheses_comment`

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:257 on 2023-08-07 07:50_

that would be the main question for me to be able to review this PR

---

_@konstin requested changes on 2023-08-07 07:51_

---

_@charliermarsh reviewed on 2023-08-07 13:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:257 on 2023-08-07 13:44_

It's not possible, no.

---

_@charliermarsh reviewed on 2023-08-07 13:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 13:47_

I will look into how we can preserve this while still fixing the bug in the PR summary, but FWIW, Black also does this formatting, _unless_ the comments are type ignores: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFIAH1dAD2IimZxl1N_WlOfrjryFgvD4ScVsKPztqdHDGJUg5knO0JCdpUeAEufiQmt7zAZacPYBA_60Y2o5_0gUa0qrRrUebiwy-1h9dFYjpJKI3bTmqTD_atbbMhmkqzeioikfpq_PQL5C6EgvdfH6suhrNVSpATyrIAX7v50BhYnAAAAAGQFt7kMk4ZRAAGZAckCAAAEz53MscRn-wIAAAAABFla.

---

_Review requested from @konstin by @charliermarsh on 2023-08-07 13:51_

---

_@charliermarsh reviewed on 2023-08-07 13:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 13:52_

(I think this should be investigated separately from this PR.)

---

_@charliermarsh reviewed on 2023-08-07 13:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 13:54_

This change seems to do what's described but it is a deviation from Black:

```diff
--- a/crates/ruff_python_formatter/src/comments/format.rs
+++ b/crates/ruff_python_formatter/src/comments/format.rs
@@ -259,11 +259,10 @@ impl Format<PyFormatContext<'_>> for FormatDanglingOpenParenthesisComments<'_> {

             write!(
                 f,
-                [line_suffix(&format_args![
-                    space(),
-                    space(),
-                    format_comment(comment)
-                ])]
+                [
+                    line_suffix(&format_args![space(), space(), format_comment(comment)]),
+                    expand_parent()
+                ]
             )?;
```

---

_@charliermarsh reviewed on 2023-08-07 13:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 13:54_

Will put up a separate PR.

---

_@konstin approved on 2023-08-07 13:55_

---

_@konstin reviewed on 2023-08-07 13:57_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:253 on 2023-08-07 13:57_

do we need this loop or can we assert that this is only one comment?

---

_@MichaReiser reviewed on 2023-08-07 13:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 13:57_

One feature that Ruff's architecture enables is that we can move comments. This is much harder in Black. I think we should use it and be opinionated about where comments should be placed and how they get formatted. 

---

_@charliermarsh reviewed on 2023-08-07 13:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:253 on 2023-08-07 13:58_

It should be _at most_ one.

---

_Merged by @charliermarsh on 2023-08-07 14:15_

---

_Closed by @charliermarsh on 2023-08-07 14:15_

---

_Branch deleted on 2023-08-07 14:15_

---

_@charliermarsh reviewed on 2023-08-07 14:16_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__multiline_consecutive_open_parentheses_ignore.py.snap`:46 on 2023-08-07 14:16_

Follow-up here: https://github.com/astral-sh/ruff/pull/6389

---
