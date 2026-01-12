```yaml
number: 3557
title: Fix snapshots missing iterations
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: fix-incomplete-snapshots
created_at: 2023-03-16T10:43:08Z
updated_at: 2023-04-30T19:48:05Z
url: https://github.com/astral-sh/ruff/pull/3557
synced_at: 2026-01-12T15:55:13Z
```

# Fix snapshots missing iterations

---

_@JonathanPlasse_

Until now the snapshots only  contained the first iteration of diagnostics.
This PR adds the diagnostics from the other iterations.
The duplicates between the iterations are removed.

---

_Comment by @github-actions[bot] on 2023-03-16 10:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.6±0.34ms     2.6 MB/sec    1.00     15.4±0.30ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.07ms     4.0 MB/sec    1.00      4.0±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   572.1±11.62µs     5.2 MB/sec    1.00   555.9±10.03µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.14ms     3.7 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.16ms     4.9 MB/sec    1.00      8.2±0.16ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1845.8±43.56µs     9.0 MB/sec    1.00  1848.3±40.19µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.07    215.9±3.73µs    13.7 MB/sec    1.00    202.3±5.54µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.10ms     6.5 MB/sec    1.00      3.9±0.07ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.9±0.48ms     2.3 MB/sec    1.02     18.3±0.53ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.26ms     3.4 MB/sec    1.01      5.0±0.19ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   629.2±23.47µs     4.7 MB/sec    1.00   616.0±23.12µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.1±0.26ms     3.1 MB/sec    1.00      8.0±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.42ms     4.2 MB/sec    1.00      9.8±0.37ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.8 MB/sec    1.02      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   234.6±11.30µs    12.6 MB/sec    1.02    239.4±8.91µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.19ms     5.5 MB/sec    1.01      4.6±0.23ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fix incomplete snapshots" to "Fix snapshots missing iterations" by @JonathanPlasse on 2023-03-16 19:31_

---

_Comment by @charliermarsh on 2023-03-16 23:27_

Hmm, this may have the potential to confuse, since the diagnostics that are generated _after_ the first iteration will contain locations that don't map to the input file (assuming the input file is changed via autofix).

Is there an example rule or set of rules for which we missed something before, but have improved diagnostic coverage after this change?

---

_Review requested from @charliermarsh by @charliermarsh on 2023-03-16 23:28_

---

_Comment by @JonathanPlasse on 2023-03-17 06:42_

- Yes, I separated in two commits #3564, where there is the before and after snapshot where the former had three fixes compared to the latter that had only two.
- The format of the snapshots, could be changed to contain an array of the iterations.

---

_Comment by @JonathanPlasse on 2023-03-17 07:33_

- Here is the diff of #3564 between the two commits.
```diff
  ---
  source: crates/ruff/src/rules/pydocstyle/mod.rs
  expression: diagnostics
  ---
  - - kind:
        name: NewLineAfterLastParagraph
        body: Multi-line docstring closing quotes should be on a separate line
        suggestion: Move closing quotes to new line
        fixable: true
      location:
        row: 2
        column: 4
      end_location:
        row: 3
        column: 72
      fix:
        content: "\n    "
        location:
          row: 3
          column: 69
        end_location:
          row: 3
          column: 69
      parent: ~
    - kind:
        name: EndsInPeriod
        body: First line should end with a period
        suggestion: Add period
        fixable: true
      location:
        row: 2
        column: 4
      end_location:
        row: 3
        column: 72
      fix:
        content: "."
        location:
          row: 3
          column: 69
        end_location:
          row: 3
          column: 69
      parent: ~
- - - kind:
-       name: NewLineAfterLastParagraph
-       body: Multi-line docstring closing quotes should be on a separate line
-       suggestion: Move closing quotes to new line
-       fixable: true
-     location:
-       row: 2
-       column: 4
-     end_location:
-       row: 4
-       column: 8
-     fix:
-       content: "\n    "
-       location:
-         row: 4
-         column: 5
-       end_location:
-         row: 4
-         column: 5
-     parent: ~
```

---

_Comment by @JonathanPlasse on 2023-03-17 07:37_

I do not know why the CI is failing, the tests are passing locally.

---

_Comment by @MichaReiser on 2023-03-17 08:11_

> I do not know why the CI is failing, the tests are passing locally.

The failing tests are gated behind the `logical_lines` feature [[source](https://github.com/charliermarsh/ruff/blob/838832a87d17f5ce5b9d642c511fed0fbab03b40/crates/ruff/src/rules/pycodestyle/mod.rs#L82-L119)].

You can run them locally with 

```
cargo insta test --all-features
```

@charliermarsh I wonder if we should remove the feature and instead rely on `cfg(debug_assertions)` or disable the feature for release builds [stack overflow](https://stackoverflow.com/questions/69428144/can-i-activate-a-dependencys-feature-only-for-debug-profile) to avoid this confusion in the future.

---

_Comment by @JonathanPlasse on 2023-03-20 22:54_

@charliermarsh, do you want to merge this PR or should we close it?

---

_Comment by @charliermarsh on 2023-03-21 00:18_

@JonathanPlasse - I do see the value, but thinking on it... I'd prefer to close, I'm just worried about making the snapshots more difficult to follow since they now represent a sequence of iterative changes. I'm sorry for not saying so sooner.

---

_Comment by @JonathanPlasse on 2023-03-21 00:21_

I understand, no problem. I could understand better how the tests work.

---

_Closed by @JonathanPlasse on 2023-03-21 00:21_

---

_Branch deleted on 2023-04-30 19:48_

---
