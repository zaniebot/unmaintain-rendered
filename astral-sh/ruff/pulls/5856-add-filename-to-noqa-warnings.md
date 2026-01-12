```yaml
number: 5856
title: "Add filename to `noqa` warnings"
type: pull_request
state: merged
author: sobolevn
labels: []
assignees: []
merged: true
base: main
head: issue-5855
created_at: 2023-07-18T06:47:29Z
updated_at: 2023-07-18T16:44:24Z
url: https://github.com/astral-sh/ruff/pull/5856
synced_at: 2026-01-12T15:55:19Z
```

# Add filename to `noqa` warnings

---

_@sobolevn_

## Summary

Before:

```
» ruff litestar tests --fix
warning: Invalid `# noqa` directive on line 19: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 65: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 74: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 22: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 66: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 75: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

After:

```
» cargo run --bin ruff ../litestar/litestar ../litestar/tests
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff ../litestar/litestar ../litestar/tests`
warning: Detected debug build without --no-cache.
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_bigint.py:19: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_bigint.py:65: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_bigint.py:74: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_uuid.py:22: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_uuid.py:66: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on /Users/sobolev/Desktop/litestar/tests/unit/test_contrib/test_sqlalchemy/models_uuid.py:75: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

## Test Plan

I didn't find any existing tests with this warning.

Closes https://github.com/astral-sh/ruff/issues/5855
Closes https://github.com/astral-sh/ruff/issues/5729

---

_Comment by @github-actions[bot] on 2023-07-18 07:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.3±0.47ms     3.3 MB/sec    1.02     12.5±0.49ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.5±0.14ms     6.8 MB/sec    1.00      2.4±0.12ms     6.9 MB/sec
formatter/numpy/globals.py                 1.01   283.7±13.05µs    10.4 MB/sec    1.00   281.1±20.07µs    10.5 MB/sec
formatter/pydantic/types.py                1.03      5.4±0.21ms     4.7 MB/sec    1.00      5.3±0.27ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.65ms     2.3 MB/sec    1.00     17.9±0.72ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec    1.00      4.4±0.18ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   585.9±21.99µs     5.0 MB/sec    1.02   594.8±40.35µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.1±0.27ms     3.1 MB/sec    1.00      8.1±0.35ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02      9.0±0.34ms     4.5 MB/sec    1.00      8.8±0.28ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1947.1±76.71µs     8.6 MB/sec    1.00  1835.6±83.09µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.1±10.28µs    13.7 MB/sec    1.02   220.1±12.37µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.15ms     6.6 MB/sec    1.03      4.0±0.14ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.08ms     3.7 MB/sec    1.01     11.2±0.07ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    231.8±4.16µs    12.7 MB/sec    1.00    231.0±5.28µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.09ms     2.6 MB/sec    1.00     15.4±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.4±7.26µs     7.0 MB/sec    1.00    423.2±7.52µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.6 MB/sec    1.00      7.0±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.04ms     5.0 MB/sec    1.00      8.1±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1640.8±13.53µs    10.1 MB/sec    1.00  1620.1±11.89µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.3±2.24µs    17.1 MB/sec    1.00    172.3±1.87µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-18 13:54_

---

_Comment by @charliermarsh on 2023-07-18 13:57_

Thank you!

---

_Merged by @charliermarsh on 2023-07-18 14:08_

---

_Closed by @charliermarsh on 2023-07-18 14:08_

---

_Comment by @sobolevn on 2023-07-18 16:27_

@charliermarsh I think that you've missed this `{path}` formatting: https://github.com/astral-sh/ruff/pull/5856/files#diff-6bf3042a2fd2080c73f4441e878fa4658bbd973395c78f30a0db0d62c92fd98dR239

I will send a PR to also make it relative.

By the way, since the last time I've contributed - you made a *huge* progress. Congrats! Keep up the good work 🤗 

---

_Comment by @charliermarsh on 2023-07-18 16:44_

Oops, serves me right for making quick drive-by changes...

> By the way, since the last time I've contributed - you made a huge progress. Congrats! Keep up the good work 🤗

Thank you so much, I really appreciate it! It's fun to see you back :)


---
