```yaml
number: 4582
title: Add a PR template
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: pr_template
created_at: 2023-05-22T15:40:54Z
updated_at: 2023-05-24T13:05:28Z
url: https://github.com/astral-sh/ruff/pull/4582
synced_at: 2026-01-12T03:50:03Z
```

# Add a PR template

---

_Pull request opened by @evanrittenhouse on 2023-05-22 15:40_

Been seeing a lot of new contributors in Discord. Might be worth it to have a PR template - open to changing the format/contents.

---

_Comment by @github-actions[bot] on 2023-05-22 16:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.10ms     2.9 MB/sec    1.05     14.7±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.03      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.8±1.63µs     7.1 MB/sec    1.02    421.6±0.87µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.05      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.12      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1441.5±2.63µs    11.6 MB/sec    1.09   1564.9±5.28µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.0±0.34µs    18.3 MB/sec    1.05    168.2±0.23µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.10      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1015.9±0.49µs    16.4 MB/sec    1.01   1022.3±1.07µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    104.4±0.51µs    28.3 MB/sec    1.00    104.6±0.41µs    28.2 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.01      2.2±0.03ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.5±0.56ms  2036.4 KB/sec    1.00     20.0±0.53ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.14ms     3.3 MB/sec    1.00      5.0±0.15ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   594.4±20.86µs     5.0 MB/sec    1.01   601.8±33.47µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.26ms     3.0 MB/sec    1.00      8.4±0.21ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.24ms     4.2 MB/sec    1.06     10.3±2.63ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.2 MB/sec    1.01      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.8±7.66µs    13.1 MB/sec    1.01    228.0±9.82µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.15ms     5.9 MB/sec    1.01      4.3±0.16ms     5.9 MB/sec
parser/large/dataset.py                    1.01      7.9±0.22ms     5.1 MB/sec    1.00      7.9±0.16ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1493.9±53.23µs    11.1 MB/sec    1.01  1510.8±59.27µs    11.0 MB/sec
parser/numpy/globals.py                    1.01    155.4±8.73µs    19.0 MB/sec    1.00    153.1±7.77µs    19.3 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.08ms     7.7 MB/sec    1.02      3.4±0.12ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @sladyn98 on `.github/PULL_REQUEST_TEMPLATE.md`:7 on 2023-05-23 20:02_

```suggestion
## Summary of changes
```

---

_Review comment by @sladyn98 on `.github/PULL_REQUEST_TEMPLATE.md`:9 on 2023-05-23 20:02_

```suggestion
<!--Provide a detailed summary of the changes you have made. This could be in the form of bullet points, detailing the changes to each file, or a paragraph form. -->
```

---

_Review comment by @sladyn98 on `.github/PULL_REQUEST_TEMPLATE.md`:13 on 2023-05-23 20:03_

## Documentation
<!-- If the PR introduces changes that need updating to the documentation, describe those changes here -->

---

_@sladyn98 reviewed on 2023-05-23 20:03_

---

_Comment by @sladyn98 on 2023-05-23 20:04_

I think we can make it a bit more detailed and merge this, it would be really great for a start considering the increase in contributors

---

_Merged by @charliermarsh on 2023-05-24 02:17_

---

_Closed by @charliermarsh on 2023-05-24 02:17_

---

_Comment by @MichaReiser on 2023-05-24 12:26_

I love this. The explicit call out for a test plan has been a good reminder for myself to add tests (or I would have to admit that I didn't add any, which I don't want to).

---

_Branch deleted on 2023-05-24 13:05_

---
