---
number: 5962
title: Airflow Benchmark Regression
type: issue
state: closed
author: ibraheemdev
labels:
  - performance
assignees: []
created_at: 2024-08-09T16:51:36Z
updated_at: 2025-11-21T17:28:28Z
url: https://github.com/astral-sh/uv/issues/5962
synced_at: 2026-01-10T01:23:54Z
---

# Airflow Benchmark Regression

---

_Issue opened by @ibraheemdev on 2024-08-09 16:51_

```
hyperfine "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20" "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08" 
Benchmark 1: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20
  Time (mean ± σ):     197.6 ms ±   5.4 ms    [User: 216.5 ms, System: 134.4 ms]
  Range (min … max):   190.1 ms … 208.2 ms    15 runs
 
Benchmark 2: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08
  Time (mean ± σ):     352.2 ms ±   8.5 ms    [User: 493.8 ms, System: 197.9 ms]
  Range (min … max):   344.0 ms … 370.4 ms    10 runs
 
Summary
  uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 ran
    1.78 ± 0.07 times faster than uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08
```

There's a similar regression with `--universal`. Spotted in https://github.com/astral-sh/uv/issues/5962.

---

_Label `performance` added by @ibraheemdev on 2024-08-09 16:51_

---

_Comment by @notatallshaw on 2024-08-09 19:06_

Given that a small change in dependencies could lead to exploring a large part of the resolution graph that doesn't lead to a direct solution, is there a reason you would expect benchmarks to remain stable across dates?

---

_Comment by @ibraheemdev on 2024-08-09 19:18_

We don't necessarily expect it to remain stable, but it's worth investigating if we can do better. As a hint, Poetry actually outperforms us on this benchmark now.

---

_Comment by @zanieb on 2024-08-09 19:18_

No, but we want to investigate the cause to see if we can do anything for it.

---

_Comment by @notatallshaw on 2024-08-09 20:42_

Well, in that case,  I see the min time between 2024-07-19 and 2024-07-20 jump by 35%.

And the output diff is quite small:

```
$ diff old.txt new.txt
2c2
< #    uv pip compile airflow.in --exclude-newer 2024-07-19 --output-file old.txt
---
> #    uv pip compile airflow.in --exclude-newer 2024-07-20 --output-file new.txt
1375c1375
< openai==1.36.0
---
> openai==1.36.1
1714c1714
< pyopenssl==24.1.0
---
> pyopenssl==24.2.1
```

The metadata in the openai wheel is almost identical, but the metadata in the pyOpenSSL has a change in an upper bound, which in my experience upper bounds are hard for resolvers and uv's main weakness in resolution.

```
$ diff pyOpenSSL-24.1.0-py3-none-any.whl.metadata pyOpenSSL-24.2.1-py3-none-any.whl.metadata
3c3
< Version: 24.1.0
---
> Version: 24.2.1
30c30
< Requires-Dist: cryptography <43,>=41.0.5
---
> Requires-Dist: cryptography <44,>=41.0.5
88a89,122
<<< A bunch of changelog notes >>>
```

In both versions of resolution cryptography gets resolved to 42.0.8 but on PyPi it is at 43.0.0: https://pypi.org/project/cryptography, so since pyOpenSSL-24.2.1 it appears uv is spending a bunch of time exploring a solution with cryptography  43.0.0 and it eventually leads to a backtrack.

Hope that helps.


---

_Comment by @notatallshaw on 2024-08-09 22:18_

I investigated this a little further by pinning apache-airflow (which is far more realistic of its use) and cryptography:

```
apache-airflow[all]==2.9.3
apache-airflow-providers-apache-beam>3.0.0
cryptography==43.0.0
```

It resolves as impossible, and ends with:

```
And because apache-airflow[all]==2.9.3 depends on apache-airflow-providers-snowflake, we can conclude that apache-airflow[all]==2.9.3 depends on cryptography>=2.5.0,<43.0.0.
  	And because you require apache-airflow[all]==2.9.3 and cryptography==43.0.0, we can conclude that the requirements are unsatisfiable.
```

I _suspect_ what you will find is that while uv pins `cryptography==43.0.0` it tries to exhaust all versions of `apache-airflow-providers-snowflake` because of `apache-airflow-providers-snowflake`s upper bound on cryptography.

I will again stand by my idea that when resolution hits an upper bound that is below the current pinned version of that upper bound, it is worth going back to an earlier state in the resolution and prioritise the package that is applying the upper bound over the package constrained by the upper bound (rather than exhausting the package that applying the upper bound): https://github.com/astral-sh/uv/issues/1398#issuecomment-2013504447 (although a lot of details need to be worked out, I haven’t had a chance recently to work on this).

There may be low hanging heuristics here though that uv could apply before something so complex.

---

_Comment by @notatallshaw on 2025-11-21 16:26_

I assume this can be closed now? Many resolver improvements have happened in uv, I don't know if this example is helpful anymore.

---

_Comment by @zanieb on 2025-11-21 16:41_

cc @konstin 

---

_Comment by @konstin on 2025-11-21 17:28_

Looks good to me:

```
$ hyperfine \
    "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20" \
    "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08" --warmup 2
Benchmark 1: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20
  Time (mean ± σ):      93.9 ms ±   3.7 ms    [User: 96.9 ms, System: 140.7 ms]
  Range (min … max):    86.6 ms … 100.6 ms    29 runs
 
Benchmark 2: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08
  Time (mean ± σ):     110.0 ms ±   5.4 ms    [User: 119.4 ms, System: 148.0 ms]
  Range (min … max):   100.4 ms … 120.3 ms    25 runs
 
Summary
  uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 ran
    1.17 ± 0.07 times faster than uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-08-08
```

A variation over time in this range is expected, because there's new versions in the set over times, but sometimes it also gets faster as conflicts are resolved in new versions.

---

_Closed by @konstin on 2025-11-21 17:28_

---
