```yaml
number: 4473
title: Allow shebang comments at start-of-file
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/shebang
created_at: 2023-05-17T14:56:35Z
updated_at: 2023-05-17T16:49:02Z
url: https://github.com/astral-sh/ruff/pull/4473
synced_at: 2026-01-12T03:50:03Z
```

# Allow shebang comments at start-of-file

---

_Pull request opened by @charliermarsh on 2023-05-17 14:56_

## Summary

pycodestyle includes some logic to allow shebang comments at the start of the file, like:

```
#!/usr/local/python
# This is fine.
#! This is an error.
```

Our existing logic didn't quite work as expected here, for a few reasons.

(1) We used `prev_line` to detect whether we're in the "first logical line", but `prev_line` isn't set for comment-only lines:

```rust
if !line.is_comment_only() {
    prev_line = Some(line);
    prev_indent_level = Some(indent_level);
}
```

(2) The actual enforcement is such that the shebang needs to be the first _physical_ line, not the first _logical_ line. So we'd allow shebangs that were preceded by arbitrary whitespace.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-17 14:56_

---

_Comment by @github-actions[bot] on 2023-05-17 15:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.00     14.1±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.5±0.86µs     7.0 MB/sec    1.00    422.5±0.93µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1454.3±3.55µs    11.4 MB/sec    1.00   1455.0±1.30µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±1.34µs    18.0 MB/sec    1.01    165.1±2.79µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.3±0.01ms     7.6 MB/sec    1.05      5.6±0.01ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1047.2±0.65µs    15.9 MB/sec    1.01   1060.6±0.97µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.2±0.17µs    27.3 MB/sec    1.02    110.6±0.22µs    26.7 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.01      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.1±0.48ms  1973.5 KB/sec    1.07     22.6±1.29ms  1839.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.16ms     3.1 MB/sec    1.04      5.5±0.31ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   640.2±29.06µs     4.6 MB/sec    1.00   632.9±25.16µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.26ms     2.9 MB/sec    1.00      8.9±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.5±0.33ms     3.9 MB/sec    1.00     10.4±0.35ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.09ms     7.4 MB/sec    1.00      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.03   265.3±29.28µs    11.1 MB/sec    1.00   256.7±14.68µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.0±0.21ms     5.1 MB/sec    1.00      4.7±0.27ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.5±0.16ms     4.8 MB/sec    1.00      8.5±0.31ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1621.5±59.84µs    10.3 MB/sec    1.03  1673.0±123.60µs    10.0 MB/sec
parser/numpy/globals.py                    1.00    165.5±5.90µs    17.8 MB/sec    1.00    165.3±5.93µs    17.9 MB/sec
parser/pydantic/types.py                   1.01      3.6±0.11ms     7.1 MB/sec    1.00      3.5±0.13ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-17 15:27_

---

_Merged by @charliermarsh on 2023-05-17 16:32_

---

_Closed by @charliermarsh on 2023-05-17 16:32_

---

_Branch deleted on 2023-05-17 16:32_

---
