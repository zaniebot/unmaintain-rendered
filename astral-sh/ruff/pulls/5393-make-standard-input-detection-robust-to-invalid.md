```yaml
number: 5393
title: Make standard input detection robust to invalid arguments
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stdin
created_at: 2023-06-27T15:58:46Z
updated_at: 2023-06-28T15:10:06Z
url: https://github.com/astral-sh/ruff/pull/5393
synced_at: 2026-01-12T03:36:55Z
```

# Make standard input detection robust to invalid arguments

---

_Pull request opened by @charliermarsh on 2023-06-27 15:58_

## Summary

This PR fixes a silent failure that manifested itself in https://github.com/astral-sh/ruff-vscode/issues/238. In short, if the user provided invalid arguments to Ruff in the VS Code extension (like `"ruff.args": ["a"]`), then we generated something like the following command:

```console
/path/to/ruff --force-exclude --no-cache --no-fix --format json - --fix a --stdin-filename /path/to/file.py
```

Since this contains both `-` and `a` as the "input files", Ruff would treat this as if we're linting the files names `-` and `a`, rather than linting standard input.

This PR modifies out standard input detection to force standard input when `--stdin-filename` is present, or at least one file is `-`. (We then warn and ignore the others.)


---

_Review requested from @konstin by @charliermarsh on 2023-06-27 15:58_

---

_Comment by @github-actions[bot] on 2023-06-27 16:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.03ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1729.7±2.22µs     9.6 MB/sec    1.01   1745.1±5.77µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    186.9±0.40µs    15.8 MB/sec    1.01    188.9±0.43µs    15.6 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.01ms     6.5 MB/sec    1.00      3.9±0.02ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.10ms     2.9 MB/sec    1.01     13.9±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.02      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.1±2.20µs     8.0 MB/sec    1.00    368.3±0.99µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.01      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1478.3±3.42µs    11.3 MB/sec    1.00   1476.8±3.53µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±0.38µs    18.7 MB/sec    1.00    157.6±0.44µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.05ms     4.3 MB/sec    1.02      9.5±0.17ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1961.2±12.87µs     8.5 MB/sec    1.02  1991.4±36.78µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    212.5±1.85µs    13.9 MB/sec    1.04   221.5±32.30µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.8 MB/sec    1.03      4.6±0.29ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.09ms     2.6 MB/sec    1.00     15.5±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.03ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    434.6±7.56µs     6.8 MB/sec    1.00    428.5±5.07µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.21ms     3.6 MB/sec    1.00      7.0±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.05ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.4±16.82µs     9.8 MB/sec    1.00  1690.5±14.84µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.2±3.08µs    16.5 MB/sec    1.04   187.0±16.69µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.02      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-28 05:17_

i'd go even further and error if stdin filename exists and files are not `["-"]`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:335 on 2023-06-28 10:20_

How do other tools handle the case where you specify multiple file names including the `-`?

I'm asking because my expectation would be that ruff treats `-` as a file if you run `ruff a - b`. 

Overall, it's problematic that `-` is ambiguous and are leaning towards always requiring `--stdin-filename`. 

---

_@MichaReiser approved on 2023-06-28 10:20_

---

_@charliermarsh reviewed on 2023-06-28 13:10_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:335 on 2023-06-28 13:10_

It looks like ESLint has a `--stdin` flag, we should... probably do that?

---

_Comment by @charliermarsh on 2023-06-28 13:10_

Going to change this to base detection on: `--stdin-filename` _or_ filenames is exactly `["-"]`, then error if you have other filenames present.

---

_Merged by @charliermarsh on 2023-06-28 14:52_

---

_Closed by @charliermarsh on 2023-06-28 14:52_

---

_Branch deleted on 2023-06-28 14:52_

---
