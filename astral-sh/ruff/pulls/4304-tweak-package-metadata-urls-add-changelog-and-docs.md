```yaml
number: 4304
title: Tweak package metadata URLs, add changelog and docs
type: pull_request
state: merged
author: intgr
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-05-09T08:52:50Z
updated_at: 2023-05-09T15:32:48Z
url: https://github.com/astral-sh/ruff/pull/4304
synced_at: 2026-01-12T15:55:15Z
```

# Tweak package metadata URLs, add changelog and docs

---

_@intgr_

These links are visible on the package's PyPI page. The lowercase "repository" is currently a bit awkward.

> ![image](https://user-images.githubusercontent.com/137616/237045248-e00245ba-58db-4788-bf99-9abd3355c94a.png)

Added "Changelog" URL, which is handy for users and is also used by tools like Renovate bot to provide direct access to the changelog for a dependency update.


---

_Review comment by @intgr on `pyproject.toml`:46 on 2023-05-09 08:54_

Perhaps also add docs link

```suggestion
Documentation = "https://beta.ruff.rs/docs/"
Changelog = "https://github.com/charliermarsh/ruff/releases"
```

---

_@intgr reviewed on 2023-05-09 08:54_

---

_Comment by @github-actions[bot] on 2023-05-09 09:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.07ms     2.7 MB/sec    1.01     15.0±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    359.8±1.16µs     8.2 MB/sec    1.00    361.4±1.29µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1530.4±3.49µs    10.9 MB/sec    1.01  1544.0±10.80µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.0±0.30µs    18.0 MB/sec    1.01    165.2±0.71µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1145.3±8.11µs    14.5 MB/sec    1.01   1152.3±1.30µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    117.1±0.46µs    25.2 MB/sec    1.01    117.8±0.28µs    25.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.26ms     2.6 MB/sec    1.01     15.5±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.08ms     4.4 MB/sec    1.00      3.8±0.07ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   451.5±16.90µs     6.5 MB/sec    1.00   444.8±14.28µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.5±0.15ms     3.9 MB/sec    1.00      6.4±0.13ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.02      7.8±0.12ms     5.2 MB/sec    1.00      7.6±0.13ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1605.1±28.76µs    10.4 MB/sec    1.00  1589.0±21.05µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.3±3.60µs    16.7 MB/sec    1.01    178.3±5.30µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.06ms     7.5 MB/sec    1.00      3.4±0.08ms     7.6 MB/sec
parser/large/dataset.py                    1.01      6.0±0.06ms     6.7 MB/sec    1.00      6.0±0.06ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1155.4±28.37µs    14.4 MB/sec    1.00  1150.3±19.71µs    14.5 MB/sec
parser/numpy/globals.py                    1.01    118.7±2.57µs    24.9 MB/sec    1.00    117.6±2.29µs    25.1 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.06ms     9.8 MB/sec    1.00      2.6±0.03ms     9.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @intgr on 2023-05-09 09:26_

If you have questions about the capitalization of URL keys, I have opened a PR against packaging.python.org docs too: https://github.com/pypa/packaging.python.org/pull/1250

---

_Review requested from @konstin by @MichaReiser on 2023-05-09 10:01_

---

_Renamed from "Tweak package metadata URLs, add changelog URL" to "Tweak package metadata URLs, add changelog and docs" by @intgr on 2023-05-09 10:11_

---

_@konstin reviewed on 2023-05-09 10:11_

---

_Review comment by @konstin on `pyproject.toml`:46 on 2023-05-09 10:11_

```suggestion
repository = "https://github.com/charliermarsh/ruff"
documentation = "https://beta.ruff.rs/docs/"
changelog = "https://github.com/charliermarsh/ruff/releases"
```

not sure how picky different tools are, but in the [docs](https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#urls) they always use lowercase keys in toml

---

_@intgr reviewed on 2023-05-09 10:12_

---

_Review comment by @intgr on `pyproject.toml`:46 on 2023-05-09 10:12_

I think those docs are wrong, I have opened a PR against it, see:

* https://github.com/pypa/packaging.python.org/pull/1250

---

_@konstin reviewed on 2023-05-09 10:13_

---

_Review comment by @konstin on `pyproject.toml`:46 on 2023-05-09 10:13_

sorry i only say your comment after looking at the code, you're likely right about this

---

_@intgr reviewed on 2023-05-09 10:13_

---

_Review comment by @intgr on `pyproject.toml`:46 on 2023-05-09 10:13_

But I know that Renovate bot does a case-insensitive comparison, so it doesn't matter for Renovate.


---

_@konstin approved on 2023-05-09 10:14_

---

_Review comment by @konstin on `pyproject.toml`:46 on 2023-05-09 10:17_

fwiw warehouse also does case insensitive checks: https://github.com/pypi/warehouse/blob/9cb091572194a3881bd8ba5164cf78b5a87bc5df/warehouse/templates/packaging/detail.html#L20-L31

---

_@konstin reviewed on 2023-05-09 10:17_

---

_Merged by @charliermarsh on 2023-05-09 15:32_

---

_Closed by @charliermarsh on 2023-05-09 15:32_

---
