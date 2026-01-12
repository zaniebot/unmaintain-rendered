```yaml
number: 3876
title: "Supports more cases in `SIM112`"
type: pull_request
state: merged
author: kyoto7250
labels: []
assignees: []
merged: true
base: main
head: fix_SIM112
created_at: 2023-04-04T04:21:31Z
updated_at: 2023-04-04T19:49:24Z
url: https://github.com/astral-sh/ruff/pull/3876
synced_at: 2026-01-12T15:55:13Z
```

# Supports more cases in `SIM112`

---

_@kyoto7250_

I checked 46bcb1f725999540a315bc6fd44f455ec3cc38a9.

I noticed the false negatives  in `SIM112`.

```python3
# OK
os.environ.get('foo')

# False Negative
env = os.environ.get('foo')
```

I think we should check this rule with `visit_expr`.


---

_Comment by @github-actions[bot] on 2023-04-04 04:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+17, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

```diff
+ airflow/cli/commands/task_command.py:290:27: SIM112 [*] Use capitalized environment variable `EXTERNAL_EXECUTOR_ID` instead of `external_executor_id`
+ airflow/example_dags/example_passing_params_via_test_command.py:56:37: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/core/test_configuration.py:106:24: SIM112 [*] Use capitalized environment variable `AIRFLOW__VAR__BROKEN` instead of `AIRFLOW__VAR__broken`
```

</p>
</details>
<details><summary>bokeh (+7, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/_testing/util/test_env.py:53:30: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:54:20: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:57:27: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:58:20: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:60:20: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:63:27: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
+ tests/unit/bokeh/_testing/util/test_env.py:64:24: SIM112 [*] Use capitalized environment variable `FOO` instead of `foo`
```

</p>
</details>
<details><summary>cibuildwheel (+2, -0)</summary>
<p>

```diff
+ test/test_windows.py:15:30: SIM112 [*] Use capitalized environment variable `PROGRAMFILES(X86)` instead of `ProgramFiles(x86)`
+ test/test_windows.py:15:68: SIM112 [*] Use capitalized environment variable `PROGRAMFILES` instead of `ProgramFiles`
```

</p>
</details>
<details><summary>scikit-build (+2, -0)</summary>
<p>

```diff
+ skbuild/platform_specifics/windows.py:165:27: SIM112 [*] Use capitalized environment variable `PROGRAMFILES(X86)` instead of `ProgramFiles(x86)`
+ skbuild/platform_specifics/windows.py:165:66: SIM112 [*] Use capitalized environment variable `PROGRAMFILES` instead of `ProgramFiles`
```

</p>
</details>
<details><summary>zulip (+3, -0)</summary>
<p>

```diff
+ tools/lib/provision.py:397:40: SIM112 [*] Use capitalized environment variable `HTTP_PROXY` instead of `http_proxy`
+ tools/lib/provision.py:398:41: SIM112 [*] Use capitalized environment variable `HTTPS_PROXY` instead of `https_proxy`
+ tools/lib/provision.py:399:38: SIM112 [*] Use capitalized environment variable `NO_PROXY` instead of `no_proxy`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.9±0.46ms     2.3 MB/sec    1.02     18.3±0.54ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.17ms     3.7 MB/sec    1.01      4.5±0.14ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   581.9±29.55µs     5.1 MB/sec    1.02   592.0±31.80µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.23ms     3.3 MB/sec    1.03      8.0±0.34ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.21ms     4.6 MB/sec    1.02      9.1±0.19ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.11ms     8.3 MB/sec    1.00      2.0±0.06ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.05   253.9±19.53µs    11.6 MB/sec    1.00   242.7±12.88µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.25ms     6.0 MB/sec    1.00      4.2±0.11ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.8±0.24ms     2.1 MB/sec    1.00     19.7±0.29ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.06ms     3.3 MB/sec    1.00      5.0±0.07ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   594.5±12.27µs     5.0 MB/sec    1.01   598.4±12.50µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.15ms     3.0 MB/sec    1.00      8.3±0.13ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02     10.2±0.12ms     4.0 MB/sec    1.00     10.0±0.13ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.05ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    234.7±7.95µs    12.6 MB/sec    1.02   240.4±10.95µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.06ms     5.6 MB/sec    1.01      4.6±0.08ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-04-04 06:44_

---

_Merged by @charliermarsh on 2023-04-04 19:49_

---

_Closed by @charliermarsh on 2023-04-04 19:49_

---
