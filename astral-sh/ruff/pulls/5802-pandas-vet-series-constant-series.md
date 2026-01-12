```yaml
number: 5802
title: "[`pandas-vet`] series constant series"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: pandas-vet-constant-series
created_at: 2023-07-16T09:43:17Z
updated_at: 2023-07-17T06:28:15Z
url: https://github.com/astral-sh/ruff/pull/5802
synced_at: 2026-01-12T15:55:19Z
```

# [`pandas-vet`] series constant series

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implementation for https://github.com/astral-sh/ruff/issues/5588

Q1: are there any additional semantic helpers that could be used to guard this rule? Which existing rules should be similar in that respect? Can we at least check if `pandas` is imported (any pointers welcome)?
Currently, the rule flags:
```python
data = {"a": "b"}
data.nunique() == 1
```

Q2: Any pointers on naming of the rule and selection of the code? It was proposed, but not replied to/implemented in the upstream. `pandas` did accept a PR to update their cookbook to reflect this rule though.

## Test Plan

TODO:
- [X] Checking for ecosystem CI results
- [x] Test on selected [real-world cases](https://github.com/search?q=%22nunique%28%29+%3D%3D+1%22+language%3APython+&type=code)
  - [x] https://github.com/sdv-dev/SDMetrics
  - [x] https://github.com/google-research/robustness_metrics
  - [x] https://github.com/soft-matter/trackpy
  - [x] https://github.com/microsoft/FLAML/
- [ ] Add guarded test cases


---

_Renamed from "Pandas vet constant series" to "[`pandas-vet`] series constant series" by @sbrugman on 2023-07-16 09:43_

---

_Comment by @github-actions[bot] on 2023-07-16 10:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.05ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   1884.9±1.86µs     8.8 MB/sec    1.00   1873.2±2.61µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.7±0.51µs    14.0 MB/sec    1.00    210.3±2.66µs    14.0 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.3±0.07ms     3.1 MB/sec    1.00     13.2±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.1±1.04µs     6.7 MB/sec    1.00    438.7±0.91µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.03ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.03ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1400.9±4.23µs    11.9 MB/sec    1.00   1393.4±1.40µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    155.4±0.28µs    19.0 MB/sec    1.00    153.4±0.37µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.6 MB/sec    1.00      2.9±0.00ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.13ms     3.7 MB/sec    1.02     11.1±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.7 MB/sec    1.01      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    243.9±4.09µs    12.1 MB/sec    1.03   250.6±12.14µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.02      4.8±0.07ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.17ms     2.6 MB/sec    1.00     15.2±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.00      4.0±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    480.3±9.64µs     6.1 MB/sec    1.02    488.9±7.61µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.01      7.0±0.86ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.06ms     5.2 MB/sec    1.01      7.9±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1646.6±23.02µs    10.1 MB/sec    1.01  1659.1±17.09µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.8±3.98µs    16.0 MB/sec    1.03    191.0±4.28µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.3 MB/sec    1.02      3.6±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-16 19:49_

The ecosystem checks are correct, even when `pandas` is not even imported.
Luckily `nunique` is not a common method..!

ℹ️ ecosystem check **detected changes**. (+12, -0, 0 error(s))

<details><summary>SDMetrics (+6, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/column_pairs/statistical/correlation_similarity.py#L70'>sdmetrics/column_pairs/statistical/correlation_similarity.py:70:13:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/column_pairs/statistical/correlation_similarity.py#L70'>sdmetrics/column_pairs/statistical/correlation_similarity.py:70:49:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/column_pairs/statistical/correlation_similarity.py#L72'>sdmetrics/column_pairs/statistical/correlation_similarity.py:72:50:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/column_pairs/statistical/correlation_similarity.py#L73'>sdmetrics/column_pairs/statistical/correlation_similarity.py:73:60:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/multi_table/statistical/cardinality_statistic_similarity.py#L48'>sdmetrics/multi_table/statistical/cardinality_statistic_similarity.py:48:12:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/sdv-dev/SDMetrics/blob/658068372b2ab8df51b70b0046fec7a8b81725a0/sdmetrics/single_column/statistical/statistic_similarity.py#L66'>sdmetrics/single_column/statistical/statistic_similarity.py:66:12:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
</pre>

</p>
</details>
<details><summary>robustness_metrics (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/google-research/robustness_metrics/blob/64a9222e8257eabd9534fc214b3cbd5b34097ccd/robustness_metrics/projects/revisiting_calibration/plotting.py#L168'>robustness_metrics/projects/revisiting_calibration/plotting.py:168:10:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/google-research/robustness_metrics/blob/64a9222e8257eabd9534fc214b3cbd5b34097ccd/robustness_metrics/projects/revisiting_calibration/utils.py#L37'>robustness_metrics/projects/revisiting_calibration/utils.py:37:10:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
</pre>

</p>
</details>
<details><summary>trackpy (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/soft-matter/trackpy/blob/05a643992b8f1dd9b18d40a04ac49dfb1775901c/trackpy/framewise_data.py#L70'>trackpy/framewise_data.py:70:12:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/soft-matter/trackpy/blob/05a643992b8f1dd9b18d40a04ac49dfb1775901c/trackpy/refine/least_squares.py#L741'>trackpy/refine/least_squares.py:741:20:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
</pre>

</p>
</details>
<details><summary>FLAML (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/microsoft/FLAML/blob/297a1ea9e0ef2a1a4bf0fa903ff63b2d46838944/flaml/automl/data.py#L293'>flaml/automl/data.py:293:24:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
+ <a href='https://github.com/microsoft/FLAML/blob/297a1ea9e0ef2a1a4bf0fa903ff63b2d46838944/flaml/automl/time_series/ts_data.py#L397'>flaml/automl/time_series/ts_data.py:397:21:</a> PD801 Using `series.nunique()` for checking that a series is constant is inefficient
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PD801 | 12 | 12 | 0 |

Configuration used:

```python
REPOSITORIES: list[Repository] = [
    Repository("sdv-dev", "SDMetrics", "master", select="PD"),
    Repository("google-research", "robustness_metrics", "master", select="PD"),
    Repository("soft-matter", "trackpy", "master", select="PD"),
    Repository("microsoft", "FLAML", "main", select="PD"),
]
```

---

_Comment by @charliermarsh on 2023-07-17 01:35_

Thanks!

1. So, I don't think there's much we can do to guard against this right now. It's similar to the other rules, we _could_ rely on Pandas being present in the list of imports, but it's also not _required_ that Pandas is imported for users to work with Pandas DataFrames, so we'd be trading some false positives for false negatives. I'm okay with this not being a blocker regardless for this rule since we already have the same issue with the others in the pandas-vet rule set.

2. I think the rule name seems appropriate (though I removed the `pandas_` prefix from the file and method, since we tend to omit those from file + method and only include in the rule name). I kind of arbitrarily re-coded to PD101, just because it felt like "new category => bump to the next available code", but I don't really feel strongly. I'll merge to keep things moving but happy to be convinced to restore PD801 or any other code really.

---

_Label `rule` added by @charliermarsh on 2023-07-17 01:47_

---

_Merged by @charliermarsh on 2023-07-17 01:55_

---

_Closed by @charliermarsh on 2023-07-17 01:55_

---

_Branch deleted on 2023-07-17 06:28_

---
