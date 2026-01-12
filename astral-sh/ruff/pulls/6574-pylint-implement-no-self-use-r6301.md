```yaml
number: 6574
title: "[`pylint`] Implement `no-self-use` (`R6301`)"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PLR6301
created_at: 2023-08-14T22:10:19Z
updated_at: 2023-08-25T20:45:40Z
url: https://github.com/astral-sh/ruff/pull/6574
synced_at: 2026-01-12T15:55:21Z
```

# [`pylint`] Implement `no-self-use` (`R6301`)

---

_@LaBatata101_

## Summary

Checks for the presence of unused `self` parameter in methods definitions.

ref: #970 

ref: #6582 

## Test Plan

Snapshots and manual runs of  `pylint`


---

_@charliermarsh reviewed on 2023-08-17 01:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:47 on 2023-08-17 01:16_

I think this should probably exclude abstract methods. We may also want to omit functions whose body is "empty" (which we typically define as just a `pass` or `...`, with or without a docstring). You can see `unused_arguments` for an example: in the method case, we omit abstract methods, overloads, empty bodies, etc.


---

_@charliermarsh reviewed on 2023-08-17 01:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:61 on 2023-08-17 01:16_

Can we use `function_type::classify` instead here, similar to how `unused_arguments` works?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:74 on 2023-08-17 01:17_

I think I'd suggest just taking the first argument, regardless of the name, since if we know this is an instance method, then the first argument is `self` regardless of whether it's named `self`.

---

_@charliermarsh reviewed on 2023-08-17 01:17_

---

_@charliermarsh reviewed on 2023-08-17 01:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:47 on 2023-08-17 01:17_

Ideally, we'd keep this as similar to `unused_arguments` as possible, to make it easier to maintain and so that the logic stays in-sync. We could even consider making this rule a codepath in the `unused_arguments` method?

---

_@LaBatata101 reviewed on 2023-08-17 19:33_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:74 on 2023-08-17 19:33_

That's right! I completely forgot about this.

---

_@LaBatata101 reviewed on 2023-08-17 19:40_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/pylint/rules/no_self_use.rs`:47 on 2023-08-17 19:40_

> Ideally, we'd keep this as similar to `unused_arguments` as possible, to make it easier to maintain and so that the logic stays in-sync. We could even consider making this rule a codepath in the `unused_arguments` method?

Yeah, it's probably better to make it a codepath in `unused_arguments`

---

_Comment by @LaBatata101 on 2023-08-18 21:25_

So I decided to keep it in `no_self_use`  instead of adding in `unused_arguments` because I couldn't find a clear way of adding it in `unused_arguments`.  The new implementation is basically the same of  `unused_arguments`, only with the checking logic (for abstract methods, overloads, empty bodies, etc.)  changed, so we can early return.

I think there is a bug in `visibility::is_override`, it's returning false for this function:
```python
class Sub(Base):
    @override
    def abstract_method(self):
        print("concret method")
```
But I didn't find the cause, so it's generating a false positive for that code.

Should we extend this rule for class methods too?

---

_Review requested from @charliermarsh by @LaBatata101 on 2023-08-18 21:29_

---

_Comment by @charliermarsh on 2023-08-22 01:37_

I think the issue there is that `@override` has to be imported from `typing` -- it's not a builtin.

---

_Label `rule` added by @charliermarsh on 2023-08-22 02:54_

---

_Comment by @github-actions[bot] on 2023-08-22 03:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.02ms    11.9 MB/sec    1.00      3.4±0.02ms    11.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    713.8±3.69µs    23.3 MB/sec    1.00    715.9±2.18µs    23.3 MB/sec
formatter/numpy/globals.py                 1.00     77.0±0.28µs    38.3 MB/sec    1.01     77.9±4.13µs    37.9 MB/sec
formatter/pydantic/types.py                1.00  1383.9±11.96µs    18.4 MB/sec    1.01   1398.8±6.42µs    18.2 MB/sec
linter/all-rules/large/dataset.py          1.01     10.1±0.04ms     4.0 MB/sec    1.00     10.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    387.6±1.00µs     7.6 MB/sec    1.00    386.2±0.58µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.06ms     4.8 MB/sec    1.00      5.3±0.02ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.02ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1200.8±3.04µs    13.9 MB/sec    1.00   1189.1±1.37µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    138.3±0.88µs    21.3 MB/sec    1.00    137.5±0.23µs    21.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.5±0.02ms    10.3 MB/sec    1.00      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.06ms    10.9 MB/sec    1.01      3.8±0.06ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   772.4±10.76µs    21.6 MB/sec    1.00   772.4±10.85µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     80.2±1.16µs    36.8 MB/sec    1.01     81.4±1.85µs    36.3 MB/sec
formatter/pydantic/types.py                1.00  1531.6±23.43µs    16.7 MB/sec    1.01  1545.8±19.42µs    16.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.14ms     3.3 MB/sec    1.01     12.5±0.11ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.9 MB/sec    1.01      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.3±6.01µs     6.8 MB/sec    1.00    434.5±6.12µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.01      6.5±0.11ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.09ms     5.8 MB/sec    1.00      7.0±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1481.3±21.22µs    11.2 MB/sec    1.00  1487.5±18.02µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.0±2.46µs    17.4 MB/sec    1.04    176.0±2.87µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-22 03:44_

---

_Closed by @charliermarsh on 2023-08-22 03:44_

---

_Branch deleted on 2023-08-25 20:45_

---
