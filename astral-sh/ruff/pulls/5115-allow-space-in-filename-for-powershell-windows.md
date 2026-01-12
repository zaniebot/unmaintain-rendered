```yaml
number: 5115
title: Allow space in filename for powershell + windows + python module
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix_subprocess_invocation_on_windows
created_at: 2023-06-15T07:48:45Z
updated_at: 2023-06-15T18:57:02Z
url: https://github.com/astral-sh/ruff/pull/5115
synced_at: 2026-01-12T15:55:17Z
```

# Allow space in filename for powershell + windows + python module

---

_@konstin_

Fixes #5077

## Summary

Previously, in a powershell on windows when using `python -m ruff` instead of `ruff` a call such as `python -m ruff "a b.py"` would fail because the space would be split into two arguments.

The python docs [recommend](https://docs.python.org/3/library/os.html#os.spawnv) using subprocess instead of os.spawn variants, which does fix the problem.

## Test Plan

I manually confirmed that the problem previously occurred and now doesn't anymore. This only happens in a very specific environment (maturin build, windows, powershell), so i could try adding a step on CI for it but i don't think it's worth it.

```
(.venv) PS C:\Users\Konstantin\PycharmProjects\ruff> python -m ruff "a b.py"
warning: Detected debug build without --no-cache.
error: Failed to lint a: The system cannot find the file specified. (os error 2)
error: Failed to lint b.py: The system cannot find the file specified. (os error 2)
a:1:1: E902 The system cannot find the file specified. (os error 2)
b.py:1:1: E902 The system cannot find the file specified. (os error 2)
Found 2 errors.
(.venv) PS C:\Users\Konstantin\PycharmProjects\ruff> python -m ruff "a b.py"
warning: Detected debug build without --no-cache.
a b.py:2:5: F841 [*] Local variable `x` is assigned to but never used
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

---

_Comment by @github-actions[bot] on 2023-06-15 08:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1388.1±2.36µs    12.0 MB/sec    1.00   1392.7±2.45µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    137.1±1.31µs    21.5 MB/sec    1.00    137.1±0.15µs    21.5 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.8 MB/sec    1.01     14.5±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.01      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.4±1.31µs     8.1 MB/sec    1.01    367.2±4.84µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.07ms     4.1 MB/sec    1.02      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.01      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1514.5±11.20µs    11.0 MB/sec    1.00   1518.8±3.69µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.3±0.18µs    18.0 MB/sec    1.01    165.7±0.47µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      9.2±0.26ms     4.4 MB/sec    1.00      8.5±0.21ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.07  1804.3±47.03µs     9.2 MB/sec    1.00  1693.9±50.44µs     9.8 MB/sec
formatter/numpy/globals.py                 1.04    166.9±6.67µs    17.7 MB/sec    1.00    160.1±6.99µs    18.4 MB/sec
formatter/pydantic/types.py                1.07      3.7±0.16ms     7.0 MB/sec    1.00      3.4±0.08ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.8±0.27ms     2.4 MB/sec    1.01     17.0±0.33ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     4.0 MB/sec    1.02      4.3±0.17ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.7±9.29µs     6.0 MB/sec    1.02   506.0±13.02µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.15ms     3.6 MB/sec    1.04      7.4±0.16ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     5.0 MB/sec    1.03      8.5±0.18ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1765.5±47.98µs     9.4 MB/sec    1.01  1781.6±39.49µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   201.6±15.03µs    14.6 MB/sec    1.00    199.0±6.76µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @KotlinIsland on 2023-06-15 08:21_

It's not just Powershell, I think it's anything Windows 

---

_@Thomasdezeeuw approved on 2023-06-15 12:02_

---

_@charliermarsh reviewed on 2023-06-15 13:07_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:36 on 2023-06-15 13:07_

I feel like this was changed to `os.spawnv` intentionally. Can you look through the Git history to figure out why it was written this way initially?

---

_Review comment by @konstin on `python/ruff/__main__.py`:36 on 2023-06-15 13:46_

This is oldest comment i can find: https://github.com/astral-sh/ruff/pull/687/files#r1020543897 . I've used `execv` on unix in monotrail when i wanted python to take over the process completely (vs. a subcommand), but i'm rather confident changing away at least from `os.spawnv` given that the docs recommend using subprocess instead (https://docs.python.org/3/library/os.html#os.spawnv)

---

_@konstin reviewed on 2023-06-15 13:46_

---

_@charliermarsh approved on 2023-06-15 18:51_

I looked through the docs too... I think this makes sense.

---

_Comment by @charliermarsh on 2023-06-15 18:51_

I wonder if this will close #832.

---

_Comment by @charliermarsh on 2023-06-15 18:52_

I suspect it will?

---

_Comment by @konstin on 2023-06-15 18:56_

pretty sure it does

---

_Merged by @konstin on 2023-06-15 18:57_

---

_Closed by @konstin on 2023-06-15 18:57_

---

_Branch deleted on 2023-06-15 18:57_

---
