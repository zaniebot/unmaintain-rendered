```yaml
number: 5172
title: "Fix corner case involving terminal backslash after fixing `W293`"
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 4404_fix_e703_w293
created_at: 2023-06-19T02:13:39Z
updated_at: 2023-06-20T03:25:07Z
url: https://github.com/astral-sh/ruff/pull/5172
synced_at: 2026-01-12T03:43:30Z
```

# Fix corner case involving terminal backslash after fixing `W293`

---

_Pull request opened by @evanrittenhouse on 2023-06-19 02:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #4404. 

Consider this file:
```python
if True:
    x = 1; \
<space><space><space>
```

The current implementation of W293 removes the 3 spaces on line 2. This fix changes the file to:
```python
if True:
    x = 1; \
```
A file can't end in a `\`, according to Python's [lexical analysis](https://docs.python.org/3/reference/lexical_analysis.html), so subsequent iterations of the autofixer fail (the AST-based ones specifically, since they depend on a valid syntax tree and get re-parsed).

 This patch examines the line before the line checked in `W293`. If its first non-whitespace character is a `\`, the patch will extend the diagnostic's fix range to all whitespace up until the previous line's *second* non-whitespace character; that is, it deletes all spaces and potential `\`s up until the next non-whitespace character on the previous line.

## Test Plan
Ran `cargo run -p ruff_cli -- ~/Downloads/aa.py --fix --select W293,D100  --no-cache` against the above file. This resulted in:
```
/Users/evan/Downloads/aa.py:1:1: D100 Missing docstring in public module
Found 2 errors (1 fixed, 1 remaining).
```
The file's contents, after the fix:
```python
if True:
    x = 1;<space>
```
The `\` was removed, leaving the terminal space. The space should be handled by `Rule::TrailingWhitespace`, not `BlankLineWithWhitespace`.

---

_Comment by @github-actions[bot] on 2023-06-19 02:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1390.5±5.08µs    12.0 MB/sec    1.00   1385.5±4.83µs    12.0 MB/sec
formatter/numpy/globals.py                 1.01    136.6±0.42µs    21.6 MB/sec    1.00    135.5±0.21µs    21.8 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.2 MB/sec    1.00      2.7±0.02ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.01     14.4±0.08ms     2.8 MB/sec    1.00     14.3±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.2±1.83µs     8.1 MB/sec    1.00    365.7±1.31µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1563.5±16.23µs    10.6 MB/sec    1.00   1548.6±5.09µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.5±0.21µs    17.7 MB/sec    1.00    165.7±0.38µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.06ms     5.3 MB/sec    1.00      7.7±0.07ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1580.5±18.40µs    10.5 MB/sec    1.00  1574.3±18.99µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    148.5±3.09µs    19.9 MB/sec    1.02    151.5±5.98µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.06ms     8.1 MB/sec    1.01      3.2±0.05ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.14ms     2.5 MB/sec    1.00     16.3±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.01      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.0±9.29µs     5.9 MB/sec    1.00    499.6±7.65µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.01      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.06ms     4.9 MB/sec    1.02      8.4±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.0±14.14µs     9.6 MB/sec    1.01  1766.5±19.84µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.0±3.05µs    14.9 MB/sec    1.02    201.5±4.43µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.02      3.8±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fix corner case with terminal backslash in W293" to "Fix corner case involving terminal backslash after fixing `W293`" by @evanrittenhouse on 2023-06-19 02:31_

---

_Review comment by @konstin on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:103 on 2023-06-19 06:35_

Is the `TextSize::from(1)` also correct in the case of `\r\n`? Similarly, could that computation be simplified by using the start of `prev` plus the trimmed length as start of the range?

---

_@konstin reviewed on 2023-06-19 06:35_

---

_@evanrittenhouse reviewed on 2023-06-19 13:38_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:103 on 2023-06-19 13:38_

I've simplified the calculation - thanks. Another question is - do we want the space between `; \` removed as well? Technically, that should get removed by `Rule::TrailingWhitespace`, not `Rule::BlankLineWithWhitespace`. Curious what you think @konstin cc @charliermarsh - the latest commit leaves the terminal space (see PR description), but I can change it back if that's preferred.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-20 00:51_

---

_@charliermarsh reviewed on 2023-06-20 01:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 01:13_

There are a few subtle issues to consider here:

1. Continuation _has_ to be the last content in a line, so no need to `.trim_end()` IIUC -- I think including space after `\` is a syntax error.
2. We actually can't check `.ends_with('\\')`, because I think we'll get a false positive on this unrealistic case:

```python
# Some comment \
```

We handle this elsewhere by checking if the line is in `indexer.is_continuation`.

3. I think we need to continue removing continuations... There could be multiple contents like:

```rust
  \
  \
  <<< some content to remove >>>
```


---

_@charliermarsh reviewed on 2023-06-20 01:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 01:13_

I'm looking at generalizing some of this from `delete_stmt`, where we have the same problem.

---

_@evanrittenhouse reviewed on 2023-06-20 01:38_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 01:38_

Gotcha. Should I wait until that's done? I was looking at something related to `F401` the other day, can't find it now, but it seemed like the same issue (though this is a physical line check which doesn't seem to use `Stmt`)

---

_@charliermarsh reviewed on 2023-06-20 01:43_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 01:43_

Yeah, I think we can use the utility here: https://github.com/astral-sh/ruff/pull/5198. I can push a change to this branch.

---

_@charliermarsh reviewed on 2023-06-20 02:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 02:07_

Do you mind giving me edit permissions?

---

_@evanrittenhouse reviewed on 2023-06-20 02:11_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 02:11_

Great. Yeah, good points raised above as well. Didn't think of those.

---

_@charliermarsh reviewed on 2023-06-20 02:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 02:13_

Or feel free to edit yourself -- here's an example: https://github.com/astral-sh/ruff/pull/5199

---

_@evanrittenhouse reviewed on 2023-06-20 02:30_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:99 on 2023-06-20 02:30_

Sure - just gave edit perms. 

---

_Merged by @charliermarsh on 2023-06-20 02:57_

---

_Closed by @charliermarsh on 2023-06-20 02:57_

---

_Comment by @charliermarsh on 2023-06-20 02:57_

Thanks @evanrittenhouse!

---
