```yaml
number: 4653
title: Improve token handling
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: release_environment
created_at: 2023-05-25T09:45:04Z
updated_at: 2023-05-26T07:52:25Z
url: https://github.com/astral-sh/ruff/pull/4653
synced_at: 2026-01-12T03:50:03Z
```

# Improve token handling

---

_Pull request opened by @konstin on 2023-05-25 09:45_

## Summary

The first small change is that this puts the release job in an environment, which we can customize later e.g. for custom approvals.

@charliermarsh Could you please create an environment "release" under https://github.com/charliermarsh/ruff/settings/environments? Defaults should be fine.

The more important change is that this switches to pypi trusted publishing, e.g. removes token handling on our end altogether.

@charliermarsh if you agree with this, could you please follow https://docs.pypi.org/trusted-publishers/adding-a-publisher/ to allow trusted publishing on pypi?

## Test Plan

Cut a release (maybe a beta release) and see whether it works :no_mouth: 


---

_Review requested from @MichaReiser by @konstin on 2023-05-25 09:45_

---

_Review requested from @charliermarsh by @konstin on 2023-05-25 09:45_

---

_Comment by @github-actions[bot] on 2023-05-25 10:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.1±0.07ms     2.7 MB/sec    1.00     14.9±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.03ms     4.5 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    378.4±0.73µs     7.8 MB/sec    1.00    376.8±0.69µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.03ms     4.0 MB/sec    1.00      6.3±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.01ms     5.3 MB/sec    1.00      7.5±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1619.0±6.13µs    10.3 MB/sec    1.00   1586.0±3.40µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    175.5±0.46µs    16.8 MB/sec    1.00    174.6±3.54µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.02ms     7.4 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.8±0.00ms     7.1 MB/sec    1.03      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1133.0±1.13µs    14.7 MB/sec    1.02   1156.9±6.40µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    117.5±0.31µs    25.1 MB/sec    1.02    119.7±1.60µs    24.7 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.02      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.16ms     2.4 MB/sec    1.01     17.0±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.8 MB/sec    1.00      4.4±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.0±5.97µs     6.6 MB/sec    1.01    448.2±5.96µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.01      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.05ms     4.8 MB/sec    1.03      8.7±0.11ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1789.3±10.63µs     9.3 MB/sec    1.03  1844.1±26.03µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.4±2.21µs    14.8 MB/sec    1.02    202.9±5.56µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.02      4.0±0.05ms     6.5 MB/sec
parser/large/dataset.py                    1.01      6.8±0.15ms     6.0 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1285.1±33.28µs    13.0 MB/sec    1.00  1280.7±11.85µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.3±1.84µs    22.5 MB/sec    1.00    131.7±1.55µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.00      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @konstin on 2023-05-25 10:10_

---

_@charliermarsh approved on 2023-05-25 19:42_

Looks great -- I went ahead and did both of the things requested in the summary.

---

_Comment by @charliermarsh on 2023-05-25 19:43_

Once this is merged, I'll also delete the environment secret.

---

_Review comment by @MichaReiser on `.github/workflows/release.yaml`:412 on 2023-05-26 05:44_

Is the `verbose` intentional or is it for debugging?

---

_@MichaReiser approved on 2023-05-26 05:44_

---

_@konstin reviewed on 2023-05-26 07:52_

---

_Review comment by @konstin on `.github/workflows/release.yaml`:412 on 2023-05-26 07:52_

yes, i'd like debug output if a run ever fails (and it doesn't bother us if it succeeds)

---

_Comment by @konstin on 2023-05-26 07:52_

> Looks great -- I went ahead and did both of the things requested in the summary.

thanks

---

_Merged by @konstin on 2023-05-26 07:52_

---

_Closed by @konstin on 2023-05-26 07:52_

---

_Branch deleted on 2023-05-26 07:52_

---
