```yaml
number: 3638
title: Consider same-site fixes to be overlapping
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/overlap
created_at: 2023-03-20T23:23:12Z
updated_at: 2023-03-21T14:09:50Z
url: https://github.com/astral-sh/ruff/pull/3638
synced_at: 2026-01-12T15:55:13Z
```

# Consider same-site fixes to be overlapping

---

_@charliermarsh_

## Summary

This PR makes our autofixer a little more cautious by considering fixes that start and end at the same location to be "overlapping". Previously, a fix had to start _before_ the end of the preceding fix to be considered "overlapping".

This isn't a great fix, but it helps avoid classes of bugs like the following:

```py
if not _DEBUG:
    if _types_coroutine is None:
        wrapper = coro
    else:
        wrapper = _types_coroutine(coro)
```

Here, one rule wants to add a `return None` within the `else`, like:

```py
if not _DEBUG:
    if _types_coroutine is None:
        wrapper = coro
    else:
        wrapper = _types_coroutine(coro)
        return None
```

But another rule wants to collapse the `else`-`if` into an `IfExp`:

```py
if not _DEBUG:
    wrapper = coro if _types_coroutine is None else _types_coroutine(coro)
```

Applying both of these fixes at present yields a syntax error:

```py
if not _DEBUG:
    wrapper = coro if _types_coroutine is None else _types_coroutine(coro)
        return None
```

We can avoid these classes of collisions (removing a block _and_ adding something to the end) by considering them to be overlapping fixes, since the second fix starts at the same location at which the first fix ends.

Closes #3617.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-20 23:23_

---

_Label `bug` added by @charliermarsh on 2023-03-20 23:23_

---

_Comment by @github-actions[bot] on 2023-03-20 23:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.7±0.22ms     2.6 MB/sec    1.00     15.6±0.33ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.09ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   559.0±14.98µs     5.3 MB/sec    1.00    553.6±9.84µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.16ms     3.8 MB/sec    1.00      6.8±0.13ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.18ms     5.0 MB/sec    1.02      8.3±0.14ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1865.0±33.98µs     8.9 MB/sec    1.00  1847.6±57.64µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.10    218.1±3.20µs    13.5 MB/sec    1.00    198.1±6.63µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.09ms     6.5 MB/sec    1.00      3.8±0.10ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.7±0.30ms     2.3 MB/sec    1.00     17.3±0.30ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.15ms     3.4 MB/sec    1.00      4.8±0.16ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.03   622.9±20.75µs     4.7 MB/sec    1.00   607.7±18.63µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.0±0.21ms     3.2 MB/sec    1.00      7.8±0.32ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.05     10.1±0.24ms     4.0 MB/sec    1.00      9.6±0.30ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.2±0.07ms     7.6 MB/sec    1.00      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    256.8±9.49µs    11.5 MB/sec    1.00   251.7±15.88µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.7±0.12ms     5.5 MB/sec    1.00      4.5±0.13ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-21 10:14_

Do I understand it correctly that this only results in `Ruff` doing one additional Linter/codefix pass if there's an overlapping range, but the experience for users doesn't change? 

I looked into how Rome handles this, and we simply re-lint the file after applying each fix. This isn't as expensive in Rome because we apply the changes on the syntax tree so that it isn't necessary to re-parse the file after applying a fix. 

https://github.com/rome/tools/blob/827455139ca862b6e0c9e7b06cf6048575441897/crates/rome_service/src/file_handlers/javascript.rs#L389-L462

---

_Comment by @charliermarsh on 2023-03-21 14:09_

Yes that's exactly right -- we may do an additional pass in certain cases, but the user experience is unchanged.

---

_Merged by @charliermarsh on 2023-03-21 14:09_

---

_Closed by @charliermarsh on 2023-03-21 14:09_

---

_Branch deleted on 2023-03-21 14:09_

---
