```yaml
number: 4398
title: Update CI to test Python wheel on Linux
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: ci/4394-2
created_at: 2023-05-12T18:47:50Z
updated_at: 2023-05-12T20:27:19Z
url: https://github.com/astral-sh/ruff/pull/4398
synced_at: 2026-01-12T03:50:02Z
```

# Update CI to test Python wheel on Linux

---

_Pull request opened by @zanieb on 2023-05-12 18:47_

Follow-up to https://github.com/charliermarsh/ruff/pull/4397 to prevent regressions like #4394 per pull request.

---

_@zanieb reviewed on 2023-05-12 18:53_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:213 on 2023-05-12 18:53_

We may want to use `container: off` to run this directly on the host instead of a container (for speed?) this is closer to the release build though so 🤷‍♀️ 

https://github.com/PyO3/maturin-action

---

_Comment by @github-actions[bot] on 2023-05-12 18:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.07ms     3.0 MB/sec    1.01     13.8±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    408.3±0.88µs     7.2 MB/sec    1.01    411.6±1.92µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.01ms     4.5 MB/sec    1.01      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.1 MB/sec    1.02      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1443.0±3.16µs    11.5 MB/sec    1.00   1450.2±1.99µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.5±0.49µs    18.6 MB/sec    1.02    161.8±1.27µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.6 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1040.0±3.65µs    16.0 MB/sec    1.01   1050.7±0.70µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    105.4±0.19µs    28.0 MB/sec    1.02    107.9±0.17µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.01      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.6±0.52ms  1931.6 KB/sec    1.01     21.7±0.47ms  1917.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.22ms     3.0 MB/sec    1.00      5.5±0.19ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   641.9±23.69µs     4.6 MB/sec    1.00   642.0±30.94µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.27ms     2.8 MB/sec    1.01      9.0±0.24ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.30ms     3.9 MB/sec    1.01     10.5±0.35ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.6 MB/sec    1.00      2.2±0.06ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   255.7±10.69µs    11.5 MB/sec    1.05   267.8±20.48µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.12ms     5.6 MB/sec    1.02      4.7±0.13ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.5±0.19ms     4.8 MB/sec    1.01      8.6±0.19ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1642.6±42.91µs    10.1 MB/sec    1.00  1638.6±52.63µs    10.2 MB/sec
parser/numpy/globals.py                    1.00    168.5±8.57µs    17.5 MB/sec    1.00    168.5±6.93µs    17.5 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.07ms     7.0 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-05-12 19:33_

I'm a little confused that this built 0.0.267 and didn't fail — seems like it built from the fixes in `main`?

---

_Comment by @charliermarsh on 2023-05-12 19:35_

Sorry, what do you mean exactly? I expected this to pass as long as it includes #4399 in the upstream.

---

_Comment by @charliermarsh on 2023-05-12 20:02_

Oh yeah, I agree, this should've failed right?

---

_Comment by @zanieb on 2023-05-12 20:17_

Yeah this branches off of `52f6663089c3983617d4d7fc07b4897c4c8433d3` and doesn't include https://github.com/charliermarsh/ruff/pull/4399 but the GitHub checkout actions default behavior is to [checkout the merge commit](https://github.com/actions/checkout/issues/426) that would be created for a pull request rather than the HEAD (aka most recent) commit in the pull request so I think if I pushed my commit _after_ you merged that fix then it would run with the fix included.

tldr; this would have failed before but you merged a fix into `main` and it's probably working as intended

---

_Merged by @charliermarsh on 2023-05-12 20:27_

---

_Closed by @charliermarsh on 2023-05-12 20:27_

---
