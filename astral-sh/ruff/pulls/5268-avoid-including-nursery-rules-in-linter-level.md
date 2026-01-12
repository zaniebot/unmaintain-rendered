```yaml
number: 5268
title: Avoid including nursery rules in linter-level selectors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/nightly
created_at: 2023-06-21T19:59:34Z
updated_at: 2023-06-22T00:29:02Z
url: https://github.com/astral-sh/ruff/pull/5268
synced_at: 2026-01-12T03:43:30Z
```

# Avoid including nursery rules in linter-level selectors

---

_Pull request opened by @charliermarsh on 2023-06-21 19:59_

## Summary

Ensures that `--select PL` and `--select PLC` don't include `PLC1901`. Previously, `--select PL` _did_, because it's a "linter-level selector" (`--select PLC` is viewed as selecting the `C` prefix from `PL`), and we were missing this filtering path.

---

_Label `bug` added by @charliermarsh on 2023-06-21 19:59_

---

_Merged by @charliermarsh on 2023-06-21 20:11_

---

_Closed by @charliermarsh on 2023-06-21 20:11_

---

_Branch deleted on 2023-06-21 20:11_

---

_Comment by @github-actions[bot] on 2023-06-21 20:15_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>cibuildwheel (+1, -0)</summary>
<p>

```diff
+ cibuildwheel/options.py:168:47: RUF100 [*] Unused `noqa` directive (non-enabled: `PLC1901`)
```

</p>
</details>
<details><summary>scikit-build-core (+1, -0)</summary>
<p>

```diff
+ tests/test_fileapi.py:109:52: RUF100 [*] Unused `noqa` directive (non-enabled: `PLC1901`)
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF100 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1612.1±6.34µs    10.3 MB/sec    1.00   1611.4±4.08µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    159.2±4.30µs    18.5 MB/sec    1.00    159.0±7.66µs    18.6 MB/sec
formatter/pydantic/types.py                1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.02ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.10ms     2.6 MB/sec    1.02     16.0±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    512.3±3.34µs     5.8 MB/sec    1.00    506.5±2.53µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.01      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.03ms     5.2 MB/sec    1.02      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1726.5±5.61µs     9.6 MB/sec    1.02   1763.3±5.79µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.4±0.31µs    15.2 MB/sec    1.03    199.5±0.68µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.02      3.7±0.01ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.10ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1567.9±12.32µs    10.6 MB/sec    1.00  1554.5±11.73µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    153.4±1.75µs    19.2 MB/sec    1.00    153.1±7.10µs    19.3 MB/sec
formatter/pydantic/types.py                1.03      3.4±0.03ms     7.6 MB/sec    1.00      3.3±0.07ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.08ms     2.7 MB/sec    1.03     15.5±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.02      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    427.2±4.36µs     6.9 MB/sec    1.00    422.8±4.40µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.03ms     3.8 MB/sec    1.03      7.0±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.1 MB/sec    1.00      7.9±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1673.9±10.30µs     9.9 MB/sec    1.00  1673.4±14.70µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.0±1.45µs    16.4 MB/sec    1.00    179.1±3.21µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.00      3.7±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-22 00:23_

@henryiii - Heads up, we made `compare-to-empty-string` (`PLC1901`) require explicit opt-in. It has a lot of false positives, and while it has parity with the Pylint version, that rule too requires explicit opt-in via an extension.

I think this will cause some `# noqa` comments in a few of your projects (per the ecosystem check, above) to be unused. You can either remove those, or add `PLC1901` to your `select`. I'm happy to help with this, if you have a preference.


---

_Comment by @henryiii on 2023-06-22 00:29_

I think they will get auto-removed in the pre-commit update PRs, so it shouldn't be a problem, thanks for checking!

And the change sounds fine to me. That one could trigger incorrectly & sometime tests especially read better comparing to an empty string.

---
