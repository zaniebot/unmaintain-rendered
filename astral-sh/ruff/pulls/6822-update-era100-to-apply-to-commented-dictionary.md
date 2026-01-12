```yaml
number: 6822
title: Update ERA100 to apply to commented dictionary items with trailing comments
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/6821
created_at: 2023-08-23T17:30:19Z
updated_at: 2023-08-25T16:02:33Z
url: https://github.com/astral-sh/ruff/pull/6822
synced_at: 2026-01-12T15:55:22Z
```

# Update ERA100 to apply to commented dictionary items with trailing comments

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/6821

ERA100 was not raising on commented parts of dictionaries if it included another comment (such as a noqa clause). In cases where this comment was a noqa clause, RUF100 to be emitted since the noqa would have no effect. Here, we update ERA100 to raise even when there are trailing comments. This resolves the linked issue _and_ increases the scope of ERA100. We could narrow the regular expression to only apply to noqa comments if we do not want to expand ERA100 however I think this change is within the spirit of the rule.

---

_Comment by @github-actions[bot] on 2023-08-23 18:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -0, 0 error(s))

<details><summary>airflow (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5dfbbbbf5adf70a9814121de8706a7c36f241836/airflow/example_dags/tutorial.py#L59'>airflow/example_dags/tutorial.py:59:9:</a> ERA001 [*] Found commented-out code
+ <a href='https://github.com/apache/airflow/blob/5dfbbbbf5adf70a9814121de8706a7c36f241836/airflow/example_dags/tutorial.py#L60'>airflow/example_dags/tutorial.py:60:9:</a> ERA001 [*] Found commented-out code
+ <a href='https://github.com/apache/airflow/blob/5dfbbbbf5adf70a9814121de8706a7c36f241836/airflow/example_dags/tutorial.py#L61'>airflow/example_dags/tutorial.py:61:9:</a> ERA001 [*] Found commented-out code
+ <a href='https://github.com/apache/airflow/blob/5dfbbbbf5adf70a9814121de8706a7c36f241836/airflow/example_dags/tutorial.py#L62'>airflow/example_dags/tutorial.py:62:9:</a> ERA001 [*] Found commented-out code
</pre>

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5a8416ff6ac648cf8f960d6698d20a77838da61c/zerver/lib/push_notifications.py#L172'>zerver/lib/push_notifications.py:172:9:</a> ERA001 [*] Found commented-out code
+ <a href='https://github.com/zulip/zulip/blob/5a8416ff6ac648cf8f960d6698d20a77838da61c/zerver/lib/push_notifications.py#L173'>zerver/lib/push_notifications.py:173:9:</a> ERA001 [*] Found commented-out code
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| ERA001 | 6 | 6 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.04ms    11.5 MB/sec    1.00      3.5±0.06ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00    707.0±8.31µs    23.6 MB/sec    1.00    705.2±8.86µs    23.6 MB/sec
formatter/numpy/globals.py                 1.00     73.8±0.56µs    40.0 MB/sec    1.01     74.3±0.34µs    39.7 MB/sec
formatter/pydantic/types.py                1.00  1417.6±15.61µs    18.0 MB/sec    1.00  1424.6±10.07µs    17.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.02ms     3.9 MB/sec    1.01     10.4±0.05ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    318.3±0.83µs     9.3 MB/sec    1.00    318.5±1.98µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.03ms     4.7 MB/sec    1.00      5.4±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.01      5.6±0.02ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1175.2±4.00µs    14.2 MB/sec    1.01   1183.0±4.36µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.7±0.23µs    24.1 MB/sec    1.01    123.4±0.37µs    23.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.01      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.02ms    10.0 MB/sec    1.00      4.1±0.04ms    10.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    792.8±6.33µs    21.0 MB/sec    1.00    793.1±7.06µs    21.0 MB/sec
formatter/numpy/globals.py                 1.00     81.1±0.94µs    36.4 MB/sec    1.00     81.4±1.18µs    36.2 MB/sec
formatter/pydantic/types.py                1.00  1636.9±14.33µs    15.6 MB/sec    1.00  1639.2±14.18µs    15.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.07ms     3.2 MB/sec    1.00     12.6±0.08ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.01      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.6±5.15µs     8.1 MB/sec    1.00    366.2±5.35µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.06ms     3.8 MB/sec    1.00      6.6±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.03ms     5.8 MB/sec    1.00      7.0±0.06ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1470.7±13.38µs    11.3 MB/sec    1.00  1475.4±12.58µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.6±2.23µs    19.9 MB/sec    1.01    150.0±2.98µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.1 MB/sec    1.00      3.2±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fix interactions between ERA100 noqa comments and RUF100" to "Fix interactions between ERA100 noqa comments in dictionaries and RUF100" by @zanieb on 2023-08-23 18:36_

---

_Comment by @zanieb on 2023-08-23 19:36_

The airflow additions look good (although they're in a tutorial and definitely intentional), zulip less so but the way they wrote that is pretty specific.

---

_Marked ready for review by @zanieb on 2023-08-23 19:46_

---

_Label `bug` added by @zanieb on 2023-08-23 20:06_

---

_Renamed from "Fix interactions between ERA100 noqa comments in dictionaries and RUF100" to "Update ERA100 to apply to commented dictionary items with trailing comments" by @zanieb on 2023-08-23 20:07_

---

_@charliermarsh approved on 2023-08-25 04:27_

LGTM

---

_Merged by @zanieb on 2023-08-25 16:02_

---

_Closed by @zanieb on 2023-08-25 16:02_

---

_Branch deleted on 2023-08-25 16:02_

---
