```yaml
number: 4687
title: "Add AIR001: task variable name should be same as task_id arg"
type: pull_request
state: merged
author: jlaneve
labels:
  - rule
assignees: []
merged: true
base: main
head: air001-task-name-aligned
created_at: 2023-05-27T20:03:38Z
updated_at: 2023-05-29T18:54:08Z
url: https://github.com/astral-sh/ruff/pull/4687
synced_at: 2026-01-12T03:50:03Z
```

# Add AIR001: task variable name should be same as task_id arg

---

_Pull request opened by @jlaneve on 2023-05-27 20:03_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds the first Ruff Airflow rule: AIR001. Related to #4421.

AIR001: An Airflow task's Python variable name should be the same as the `task_id` arg you give it. This follows https://github.com/BasPH/pylint-airflow C8300.

This PR also adds the groundwork for adding new Airflow-related rules!

## Test Plan

<!-- How was it tested? -->

Unit tests included in the PR.


---

_@jlaneve reviewed on 2023-05-27 20:05_

---

_Review comment by @jlaneve on `crates/ruff/src/rules/airflow/mod.rs`:51 on 2023-05-27 20:05_

I copied this from https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/rules/pandas_vet/mod.rs#L23-L55 - makes me wonder if this was the correct way of doing things, and if so, if there's a nicer way of abstracting these unit tests to keep things in sync. I imagine the general pattern of "this code should raise these linting diagnostics" is quite common.

---

_@jlaneve reviewed on 2023-05-27 20:07_

---

_Review comment by @jlaneve on `crates/ruff/src/rules/airflow/rules/tasks.rs`:73 on 2023-05-27 20:07_

in theory this should only be run if the class being called inherits from `airflow.models.baseoperator.BaseOperator`, but I wanted to get something up to at least start a discussion. Also worth noting that I tried `checker.ctx.resolve_call_path` but couldn't get it to work... could certainly be user error though.

---

_Comment by @github-actions[bot] on 2023-05-27 20:37_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     14.9±0.04ms     2.7 MB/sec    1.00     14.1±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.5±0.00ms     4.7 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    430.8±0.39µs     6.8 MB/sec    1.00    425.4±0.77µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.1±0.01ms     4.2 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.09      7.5±0.02ms     5.4 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05   1603.0±1.95µs    10.4 MB/sec    1.00   1522.9±3.15µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    176.3±0.20µs    16.7 MB/sec    1.00    172.2±0.22µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.3±0.01ms     7.6 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.1±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1021.1±2.41µs    16.3 MB/sec    1.00   1022.0±0.40µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.9±0.20µs    27.9 MB/sec    1.00    105.5±0.34µs    28.0 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.19ms     2.5 MB/sec    1.00     16.6±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.7±5.34µs     5.9 MB/sec    1.01   503.2±13.33µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.01      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.01      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1752.8±19.64µs     9.5 MB/sec    1.01  1771.5±18.23µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.1±2.77µs    14.6 MB/sec    1.03    207.5±4.94µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.15      7.4±0.04ms     5.5 MB/sec    1.00      6.5±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.12  1374.7±15.24µs    12.1 MB/sec    1.00  1231.7±11.18µs    13.5 MB/sec
parser/numpy/globals.py                    1.08    136.1±1.68µs    21.7 MB/sec    1.00    125.8±2.02µs    23.5 MB/sec
parser/pydantic/types.py                   1.12      3.1±0.02ms     8.2 MB/sec    1.00      2.8±0.02ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @jlaneve on 2023-05-27 21:07_

@charliermarsh any chance you can help with the CI issues? Not too sure what’s going on, this works fine locally and the code looks consistent with what I see elsewhere. 

Feel free to push changes directly to this branch if it’s easier than adding comments inline!

---

_@charliermarsh reviewed on 2023-05-27 21:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/airflow/mod.rs`:51 on 2023-05-27 21:16_

We tend to do this a bit less than might be expected. More commonly, we put the test fixtures in Python files (e.g., `crates/ruff/resources/test/fixtures/flake8_2020`), and then we have a `test_path` utility that runs over files + generates a snapshot. (You could see `rules/flake8_2020/mod.rs` as an example.)

I think this kind of inline testing is fine for simple snippets, if you prefer it. We could DRY up this `rule_code` utility as a TODO for later (and perhaps change it to use snapshots rather than encoding the expected violations).

---

_Comment by @charliermarsh on 2023-05-27 21:19_

Not sure what that failure was -- we replaced `.ctx` with `.semantic_model()`, but your branch was also passing for me locally (since that change was past your branch's upstream). Maybe some kind of weird Cargo caching failure on CI? Regardless, I merged in `main` + changed the reference, so hopefully it passes now.

---

_@jlaneve reviewed on 2023-05-27 22:29_

---

_Review comment by @jlaneve on `crates/ruff/src/rules/airflow/mod.rs`:51 on 2023-05-27 22:29_

Nice, thanks for that info - planning to open up a follow-on PR documenting how the snapshots work!

---

_Marked ready for review by @jlaneve on 2023-05-27 22:29_

---

_Label `rule` added by @charliermarsh on 2023-05-29 03:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1673 on 2023-05-29 03:18_

(Unrelated, but the newlines here felt inconsistent, so just removed.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/airflow/rules/task_variable_name.rs`:39 on 2023-05-29 03:19_

I renamed this to prefix with "Airflow" to match the "Django" and "NumPy" conventions.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/airflow/rules/task_variable_name.rs`:43 on 2023-05-29 03:19_

(Added the `task_id` itself to the message.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/airflow/rules/task_variable_name.rs`:72 on 2023-05-29 03:21_

Modified this to use `resolve_call_path`, which will resolve to the fully qualified path -- so it'll do the right thing in cases like:

```py
import airflow

foo = airflow.PythonOperator(...)

import airflow as Foo

foo = Foo.PythonOperator(...)
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/airflow/rules/task_variable_name.rs`:80 on 2023-05-29 03:21_

Can use `?` to tighten this up a little.

---

_Review comment by @charliermarsh on `README.md`:294 on 2023-05-29 03:22_

I decided to link to the Pylint plugin, since this list refers to tools we've reimplemented, and it felt more appropriate than "Airflow" -- wdyt?

---

_@charliermarsh reviewed on 2023-05-29 03:23_

Looks great @jlaneve! I went ahead and pushed some changes, but commenting on them here for clarity + transparency -- let me know if any of them are incorrect, confusing, etc. Thanks for getting this started!

---

_Merged by @charliermarsh on 2023-05-29 03:25_

---

_Closed by @charliermarsh on 2023-05-29 03:25_

---

_Review comment by @jlaneve on `README.md`:294 on 2023-05-29 18:54_

yeah this makes sense to me! hopefully we get to a point where we're linting for more than just pylint-airflow though 🙂 

---

_@jlaneve reviewed on 2023-05-29 18:54_

---
