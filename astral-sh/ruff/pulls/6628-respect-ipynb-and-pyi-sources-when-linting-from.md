```yaml
number: 6628
title: Respect .ipynb and .pyi sources when linting from stdin
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/jupyter
created_at: 2023-08-16T18:43:53Z
updated_at: 2023-08-16T20:51:11Z
url: https://github.com/astral-sh/ruff/pull/6628
synced_at: 2026-01-12T02:52:04Z
```

# Respect .ipynb and .pyi sources when linting from stdin

---

_Pull request opened by @charliermarsh on 2023-08-16 18:43_

## Summary

When running Ruff from stdin, we were always falling back to the default source type, even if the user specified a path (as is the case when running from the LSP). This PR wires up the source type inference, which means we now get the expected result when checking `.pyi` and `.ipynb` files.

Closes #6627.

## Test Plan

Verified that `cat crates/ruff/resources/test/fixtures/jupyter/valid.ipynb | cargo run -p ruff_cli --  --force-exclude --no-cache --no-fix --isolated --select ALL --stdin-filename foo.ipynb -` yielded the expected results (and differs from the errors you get if you omit the filename).

Verified that `cat foo.pyi | cargo run -p ruff_cli --  --force-exclude --no-cache --no-fix --format json --isolated --select TCH --stdin-filename path/to/foo.pyi -` yielded no errors.


---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:187 on 2023-08-16 18:44_

I decided to get rid of this. It's nice but kind of hard to preserve now that this method is agnostic to path-or-string (since it takes a reader).

---

_@charliermarsh reviewed on 2023-08-16 18:44_

---

_Label `bug` added by @charliermarsh on 2023-08-16 18:44_

---

_Label `cli` added by @charliermarsh on 2023-08-16 18:44_

---

_Marked ready for review by @charliermarsh on 2023-08-16 18:44_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-16 18:44_

---

_Comment by @github-actions[bot] on 2023-08-16 18:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.14ms     9.9 MB/sec    1.02      4.2±0.16ms     9.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   840.7±24.39µs    19.8 MB/sec    1.00   840.7±26.33µs    19.8 MB/sec
formatter/numpy/globals.py                 1.01     86.8±3.90µs    34.0 MB/sec    1.00     86.1±2.34µs    34.3 MB/sec
formatter/pydantic/types.py                1.00  1689.5±39.82µs    15.1 MB/sec    1.03  1740.5±109.07µs    14.7 MB/sec
linter/all-rules/large/dataset.py          1.04     13.4±0.55ms     3.0 MB/sec    1.00     12.9±0.29ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.06ms     4.9 MB/sec    1.02      3.5±0.30ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   500.8±16.41µs     5.9 MB/sec    1.01   505.1±18.02µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.0±0.33ms     3.7 MB/sec    1.00      6.8±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      6.9±0.17ms     5.9 MB/sec    1.00      6.7±0.16ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1500.1±48.42µs    11.1 MB/sec    1.00  1453.0±34.23µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    189.3±8.29µs    15.6 MB/sec    1.00   185.4±11.19µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.11ms     8.3 MB/sec    1.00      3.0±0.09ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.9±0.35ms     8.3 MB/sec     1.06      5.2±0.28ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   952.2±44.75µs    17.5 MB/sec     1.07  1016.4±43.96µs    16.4 MB/sec
formatter/numpy/globals.py                 1.00     96.9±4.89µs    30.5 MB/sec     1.03    100.1±4.53µs    29.5 MB/sec
formatter/pydantic/types.py                1.00  1931.1±104.35µs    13.2 MB/sec    1.06      2.1±0.08ms    12.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.39ms     2.3 MB/sec     1.01     18.1±0.53ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.13ms     3.5 MB/sec     1.03      4.9±0.15ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.11   608.6±26.28µs     4.8 MB/sec     1.00   549.8±30.76µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.36ms     2.8 MB/sec     1.02      9.4±0.38ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.02      9.8±0.31ms     4.1 MB/sec     1.00      9.6±0.60ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.2 MB/sec     1.00      2.0±0.07ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   257.0±16.25µs    11.5 MB/sec     1.00   251.6±11.08µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.23ms     6.3 MB/sec     1.07      4.3±0.11ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-16 19:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-08-16 20:08_

Wouldn't it be possible to keep this logic by rewinding the reader and then reading it to a string?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:442 on 2023-08-16 20:11_

Could we instead pass an `Option<Path>` to `notebook_from_contents`? This feels like something that's easy to get wrong in the future.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:453 on 2023-08-16 20:12_

I assume your other PR deals with these many `to_string` calls?

---

_@MichaReiser approved on 2023-08-16 20:12_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:442 on 2023-08-16 20:15_

We need it to format the error message. I could restructure the code such that we only take this branch if we have a Path though.

---

_@charliermarsh reviewed on 2023-08-16 20:15_

---

_@charliermarsh reviewed on 2023-08-16 20:16_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:453 on 2023-08-16 20:16_

Yeah

---

_@charliermarsh reviewed on 2023-08-16 20:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-08-16 20:16_

No, because the reader gets consumed above by the Serde call I believe. I did try this!

---

_@charliermarsh reviewed on 2023-08-16 20:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-08-16 20:18_

Maybe possible with `reader.by_ref()`, I will try again.

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-08-16 20:25_

Restored!

---

_@charliermarsh reviewed on 2023-08-16 20:25_

---

_Merged by @charliermarsh on 2023-08-16 20:33_

---

_Closed by @charliermarsh on 2023-08-16 20:33_

---

_Branch deleted on 2023-08-16 20:34_

---
