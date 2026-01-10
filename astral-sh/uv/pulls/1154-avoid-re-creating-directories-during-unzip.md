```yaml
number: 1154
title: Avoid re-creating directories during unzip
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/dirs
created_at: 2024-01-28T05:03:46Z
updated_at: 2024-01-28T05:07:58Z
url: https://github.com/astral-sh/uv/pull/1154
synced_at: 2026-01-10T15:39:03Z
```

# Avoid re-creating directories during unzip

---

_Pull request opened by @charliermarsh on 2024-01-28 05:03_

## Summary

We have this optimization in `wheel.rs`, in the installer, but it makes a huge difference for zips with many small files:

```
Benchmarking file_reader/Django-5.0.1-py3-none-any.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 74.2s, or reduce sample count to 10.
file_reader/Django-5.0.1-py3-none-any.whl
                        time:   [751.63 ms 757.78 ms 764.27 ms]
                        change: [-1.0290% +0.0841% +1.2289%] (p = 0.88 > 0.05)
                        No change in performance detected.
Found 4 outliers among 100 measurements (4.00%)
  4 (4.00%) high mild

Benchmarking buffered_reader/Django-5.0.1-py3-none-any.whl: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 53.4s, or reduce sample count to 10.
buffered_reader/Django-5.0.1-py3-none-any.whl
                        time:   [529.86 ms 536.44 ms 543.35 ms]
                        change: [+0.0293% +1.5543% +3.1426%] (p = 0.05 > 0.05)
                        No change in performance detected.
Found 3 outliers among 100 measurements (3.00%)
  3 (3.00%) high mild
```

That's almost 30% faster...


---

_Merged by @charliermarsh on 2024-01-28 05:07_

---

_Closed by @charliermarsh on 2024-01-28 05:07_

---

_Branch deleted on 2024-01-28 05:07_

---

_Label `performance` added by @charliermarsh on 2024-01-28 05:07_

---
