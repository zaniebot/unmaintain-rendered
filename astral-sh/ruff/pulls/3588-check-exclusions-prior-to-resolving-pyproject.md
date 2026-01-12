```yaml
number: 3588
title: "Check exclusions prior to resolving `pyproject.toml` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/build
created_at: 2023-03-17T21:53:39Z
updated_at: 2023-03-18T17:12:51Z
url: https://github.com/astral-sh/ruff/pull/3588
synced_at: 2026-01-12T15:55:13Z
```

# Check exclusions prior to resolving `pyproject.toml` files

---

_@charliermarsh_

## Summary

This PR inverts the order of the "Check if this directory contains a `pyproject.toml`" and "Check if this path is included" operations.

Right now, `build` contains a subdirectory that intentionally includes a `pyproject.toml` with a syntax error (but no other files). You'd expect that:

```toml
[tool.ruff]
exclude = ["path/to/directory"]
```

...would ignore the directory, and thus avoid a parse error. However, right now, when we visit that directory, we check for a `pyproject.toml` immediately, _then_ skip recursing any further based on the current exclusions.

This PR reverse the order of operations. This _is_ a change in behavior, as previously, it was _impossible_ to exclude a directory that itself contained Ruff configuration. But if a user is excluding a directory as above, this seems like the expected and intuitive behavior.

See https://github.com/charliermarsh/ruff/pull/3569.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-17 21:57_

---

_Label `bug` added by @charliermarsh on 2023-03-17 21:57_

---

_Comment by @charliermarsh on 2023-03-17 21:57_

\cc @henryiii 

---

_Comment by @github-actions[bot] on 2023-03-17 22:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.01     13.8±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.3±3.84µs     5.9 MB/sec    1.01    504.0±1.42µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.01      7.6±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1691.7±1.55µs     9.8 MB/sec    1.01   1701.4±5.91µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.7±0.38µs    15.4 MB/sec    1.02    196.0±0.68µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.3 MB/sec    1.01      3.5±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.7±0.19ms     2.6 MB/sec    1.01     15.9±0.35ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.01      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   543.2±13.34µs     5.4 MB/sec    1.00   538.8±10.80µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.01      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.10ms     4.7 MB/sec    1.01      8.8±0.09ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1911.5±36.88µs     8.7 MB/sec    1.00  1920.2±27.99µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.0±3.71µs    14.0 MB/sec    1.02   214.2±10.42µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.06ms     6.4 MB/sec    1.02      4.1±0.08ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @henryiii on 2023-03-17 23:43_

Looks reasonable to me!

---

_@MichaReiser approved on 2023-03-18 08:35_

---

_Merged by @charliermarsh on 2023-03-18 17:12_

---

_Closed by @charliermarsh on 2023-03-18 17:12_

---

_Branch deleted on 2023-03-18 17:12_

---
