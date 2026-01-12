```yaml
number: 3680
title: "[`flake8-bugbear`]: Implement rule `B031`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/B031
created_at: 2023-03-23T09:07:08Z
updated_at: 2023-03-25T01:48:01Z
url: https://github.com/astral-sh/ruff/pull/3680
synced_at: 2026-01-12T15:55:13Z
```

# [`flake8-bugbear`]: Implement rule `B031`

---

_@dhruvmanila_

Implement rule `B031` for `flake8-bugbear`:

> Using the generator returned from `itertools.groupby()` more than once will do nothing on the second usage. Save the result to a list if the result is needed multiple times.

resolves: #2954 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:132 on 2023-03-23 09:23_

Do we need similar handling to `for` loops for other loops (while, list comprehension...)?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:85 on 2023-03-23 09:28_

Not sure how to handle it but this analysis will yield false-positives if `id` is re-bound`. For example, because a new value is assigned, the visitor enters a function that has an argument with the same name, etc. 

---

_@MichaReiser reviewed on 2023-03-23 09:28_

---

_Comment by @github-actions[bot] on 2023-03-23 09:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.05ms     2.8 MB/sec    1.00     14.1±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    439.3±1.23µs     6.7 MB/sec    1.00    435.9±2.84µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.01ms     5.2 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1713.9±3.13µs     9.7 MB/sec    1.00   1675.8±2.93µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.0±0.31µs    16.8 MB/sec    1.00    174.8±0.37µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.20ms     2.7 MB/sec    1.01     15.4±1.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    458.9±8.68µs     6.4 MB/sec    1.00    457.3±8.46µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.12ms     3.8 MB/sec    1.00      6.8±0.14ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.12ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.7±17.35µs     9.4 MB/sec    1.00  1763.0±22.67µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±1.21µs    16.3 MB/sec    1.01    183.8±4.10µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.06ms     6.6 MB/sec    1.01      3.9±0.09ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila reviewed on 2023-03-23 09:54_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:132 on 2023-03-23 09:54_

Good point! 

Right now the implementation is mainly based on the original one. But yes, I think we should also include it for other loop constructs: `while`, comprehensions.

```python
# using list comprehension
for k, g in it.groupby(a):
    val = [list(g) for _ in range(3)]
    print(val)

# using `while` loop
for k, g in it.groupby(a):
    i = 3
    while i > 0:
        print(list(g))
        i -= 1
```

---

_@dhruvmanila reviewed on 2023-03-23 09:58_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:85 on 2023-03-23 09:58_

Right! Variable re-assignment inside the loop might yield false-positives. From the top of my mind, I'm thinking of skipping the checks during the scope of the variable assignment. I'm not sure about the scope determination part but something along the lines of `self.nested` should work.

---

_Comment by @dhruvmanila on 2023-03-23 09:58_

@MichaReiser Thanks for the review! I'll look at your points and the ecosystem checks later in the evening.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:132 on 2023-03-23 10:32_

I just thought of a case where it would give false negative - when using multiple _separate_ nested loops looping over the _same_ group:

```python
for name, group in itertools.groupby(data):
	for item in group:
		pass

	for item in group:
		pass
```

The reason it will give false negative, and this is after the latest [fix commit](https://github.com/charliermarsh/ruff/pull/3680/commits/8955e3c9828d4bf89a2a30f0ec257340923c525f), the implementation skips checking the `iter` and `target` node of the `for` loop and goes directly to the `body`. This will need to be handled as well.

---

_@dhruvmanila reviewed on 2023-03-23 10:32_

---

_Comment by @dhruvmanila on 2023-03-23 10:39_

These ecosystem checks are really helpful! Thank you to whoever implemented them! ❤️ 

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-03-23 14:55_

---

_@charliermarsh approved on 2023-03-23 19:07_

LGTM, I'll let @MichaReiser approve and merge.

---

_Comment by @charliermarsh on 2023-03-24 21:26_

(Gonna merge to keep things chugging along, but @MichaReiser let me know if you have any follow-ups.)

---

_Merged by @charliermarsh on 2023-03-24 21:26_

---

_Closed by @charliermarsh on 2023-03-24 21:26_

---

_Label `rule` added by @charliermarsh on 2023-03-24 21:26_

---

_Branch deleted on 2023-03-25 01:48_

---
