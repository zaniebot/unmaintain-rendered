```yaml
number: 3659
title: "[`flake8-django`]: Implement rule DJ012"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: feat/DJ012
created_at: 2023-03-22T02:18:54Z
updated_at: 2023-03-22T03:38:11Z
url: https://github.com/astral-sh/ruff/pull/3659
synced_at: 2026-01-12T04:39:45Z
```

# [`flake8-django`]: Implement rule DJ012

---

_Pull request opened by @dhruvmanila on 2023-03-22 02:18_

Checks for the order of Model's inner classes, methods and fields as per the Django Style Guide.

Django Style Guide: https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style

resolves: #2817 

---

_@charliermarsh reviewed on 2023-03-22 02:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/helpers.rs`:27 on 2023-03-22 02:37_

Do you mind changing all of these to take `context: Context` rather than `Checker`? Since they only rely on `checker.ctx` anyway. (`Context` was newly carved out of `Checker`, and we're trying to make code that doesn't need the full `Checker` take `Context` instead.)

---

_@charliermarsh approved on 2023-03-22 02:40_

This is really thorough and impressive work!

---

_@dhruvmanila reviewed on 2023-03-22 02:50_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_django/rules/helpers.rs`:27 on 2023-03-22 02:50_

By "all of these" do you mean all of flake8-django rules or just this one?

---

_@charliermarsh reviewed on 2023-03-22 02:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/helpers.rs`:27 on 2023-03-22 02:52_

I was referring to all of the utility functions in this file, since they follow a similar pattern, but it's also totally fine if you only want to change this one function.

---

_@dhruvmanila reviewed on 2023-03-22 02:54_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_django/rules/helpers.rs`:27 on 2023-03-22 02:54_

Oh wait, I didn't see which line you were referring to.

---

_@charliermarsh reviewed on 2023-03-22 03:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/helpers.rs`:27 on 2023-03-22 03:06_

Thanks!

---

_Merged by @charliermarsh on 2023-03-22 03:07_

---

_Closed by @charliermarsh on 2023-03-22 03:07_

---

_Branch deleted on 2023-03-22 03:11_

---

_Comment by @github-actions[bot] on 2023-03-22 03:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+23, -0, 0 error(s))

<details><summary>zulip (+23, -0)</summary>
<p>

```diff
+ confirmation/models.py:185:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
+ corporate/models.py:45:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ zerver/models.py:1118:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
+ zerver/models.py:1318:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ zerver/models.py:1480:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ zerver/models.py:2632:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
+ zerver/models.py:4073:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before `Meta` class
+ zerver/models.py:4074:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before `Meta` class
+ zerver/models.py:4075:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before `Meta` class
+ zerver/models.py:4078:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before `Meta` class
+ zerver/models.py:4096:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before `Meta` class
+ zerver/models.py:4303:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ zerver/models.py:4632:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
+ zerver/models.py:691:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:693:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:697:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:755:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:760:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:769:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:774:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:776:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:781:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before custom method
+ zerver/models.py:803:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `__str__` method should come before custom method
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.5±0.51ms     2.5 MB/sec    1.00     16.1±0.59ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    576.7±4.05µs     5.1 MB/sec    1.00    571.6±3.67µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.05ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.05ms     4.8 MB/sec    1.06      8.9±0.20ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1963.0±60.51µs     8.5 MB/sec    1.02      2.0±0.05ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    222.3±8.07µs    13.3 MB/sec    1.02    226.7±8.28µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.03ms     6.4 MB/sec    1.06      4.2±0.14ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     21.7±1.13ms  1922.1 KB/sec    1.00     20.4±1.15ms  2044.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.5±0.34ms     3.0 MB/sec    1.00      5.4±0.29ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   684.0±45.94µs     4.3 MB/sec    1.00   662.8±49.60µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.48ms     2.8 MB/sec    1.00      9.1±0.56ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     11.1±0.59ms     3.7 MB/sec    1.08     12.1±0.80ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.6±0.16ms     6.5 MB/sec    1.01      2.6±0.15ms     6.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   289.3±20.09µs    10.2 MB/sec    1.00   289.9±23.58µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.3±0.23ms     4.8 MB/sec    1.03      5.4±0.32ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
