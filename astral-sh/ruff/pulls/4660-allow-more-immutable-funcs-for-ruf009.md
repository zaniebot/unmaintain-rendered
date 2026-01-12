```yaml
number: 4660
title: Allow more immutable funcs for RUF009
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: fix/fixRUF009
created_at: 2023-05-25T16:46:54Z
updated_at: 2023-05-26T19:42:48Z
url: https://github.com/astral-sh/ruff/pull/4660
synced_at: 2026-01-12T03:50:03Z
```

# Allow more immutable funcs for RUF009

---

_Pull request opened by @qdegraaf on 2023-05-25 16:46_

## Summary

`RUF009` wrongly flags on `float("-inf")` calls. As discussed in https://github.com/charliermarsh/ruff/issues/4657 we can take this moment to add more funcs with immutable results.

TODOs:

- [x] Check for completeness of added funcs
- [x] @charliermarsh to confirm that we are sure we want to allow these funcs in `flake8_bugbear` as well (as discussed in the issue). The tests for bugbear seem quite explicit about only allowing certain `float()` calls for example. If changed Ruff would appear to deviate from upstream implementation. Which might be desirable in this case, but if not, PR can be changed to add a separate constant for `RUF009`.

## Test Plan

Cases added to `RUF009.py` 
Test TODOs:

- [x] Change comments in `flake8_bugbear` tests if we end up editing its logic

Closes: https://github.com/charliermarsh/ruff/issues/4657

---

_Comment by @github-actions[bot] on 2023-05-25 16:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- setup.py:208:35: B008 Do not perform function call `str` in argument defaults
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B008 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.6±0.71ms  2026.0 KB/sec    1.00     20.3±0.64ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.16ms     3.6 MB/sec    1.01      4.7±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   589.6±26.13µs     5.0 MB/sec    1.00   583.3±25.98µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.39ms     3.1 MB/sec    1.00      8.3±0.34ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.5±0.36ms     4.3 MB/sec    1.00      9.4±0.33ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.07ms     8.1 MB/sec    1.00      2.0±0.11ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.03   239.0±10.83µs    12.3 MB/sec    1.00   233.2±11.11µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.3±0.18ms     5.9 MB/sec    1.00      4.2±0.16ms     6.1 MB/sec
parser/large/dataset.py                    1.00      7.3±0.20ms     5.6 MB/sec    1.04      7.6±0.23ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1438.2±53.12µs    11.6 MB/sec    1.04  1495.4±89.18µs    11.1 MB/sec
parser/numpy/globals.py                    1.00    146.2±8.63µs    20.2 MB/sec    1.00    146.0±6.72µs    20.2 MB/sec
parser/pydantic/types.py                   1.01      3.3±0.16ms     7.8 MB/sec    1.00      3.2±0.10ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.3±0.75ms     2.4 MB/sec    1.00     16.9±0.31ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.4±0.11ms     3.8 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   515.4±19.61µs     5.7 MB/sec    1.00   510.0±11.65µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.3±0.15ms     3.5 MB/sec    1.00      7.0±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.12ms     4.8 MB/sec    1.00      8.6±0.21ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1826.3±63.12µs     9.1 MB/sec    1.00  1791.3±50.20µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.3±9.30µs    14.0 MB/sec    1.01   214.2±14.84µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.13ms     6.4 MB/sec    1.00      3.9±0.13ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.5±0.09ms     6.2 MB/sec    1.00      6.5±0.10ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1242.7±21.93µs    13.4 MB/sec    1.01  1251.9±41.24µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    126.3±3.82µs    23.4 MB/sec    1.01    127.5±4.31µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.06ms     9.1 MB/sec    1.00      2.8±0.05ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-25 19:59_

Oh, this is interesting, I didn't realize we had special-casing for those floats... How would you feel about instead changing our other rules to respect that same carveout?

---

_Comment by @qdegraaf on 2023-05-25 20:40_

> Oh, this is interesting, I didn't realize we had special-casing for those floats... How would you feel about instead changing our other rules to respect that same carveout?

I would be happy to! Just for my understanding, and to be complete, to what degree does this apply to other primitive Python functions? The original `flake8-bugbear` rule states:

> "B008 Do not perform function calls in argument defaults.  The call is "
        "performed only once at function definition time. All calls to your "
        "function will reuse the result of that definition-time function call.  If "
        "this is intended, assign the function call to a module-level variable and "
        "use that variable as a default value."

So I take it calling `float("-inf")` or a similar infinite value avoids this necessity somehow. 

Essentially what I am wondering is: if the following default args are ok, and they do not need to be moved to module vars:

```python
def foo(x: float = float("-inf"), y: typing.Tuple[int] = tuple(1,2)):
    ...
``` 

why do these need to be moved (according to bugbear)?
```python
def bar(x: str = str("bla"), y: int = int(42)):
    ...
```

As far as I understand these primitives, they all create immutable values that are fine to be reused (or as fine as their allowed counterparts). I'm assuming it's not an oversight in bugbear or here, so I'm just checking so I can understand what the difference is and I can be certain I'm not adding or removing the wrong function to either `RUF` or `B` 

---

_Comment by @charliermarsh on 2023-05-25 20:55_

So in this case, I'd go to the source and try to find a record of why / how the decision was made...

In this case, I found https://github.com/PyCQA/flake8-bugbear/pull/155 and https://github.com/PyCQA/flake8-bugbear/issues/154. It sounds like originally, bugbear didn't allow any `float` calls (even `float("-inf")` or similar). The issue was raised to support those cases, since they don't have corresponding literal definitions, and they made the choice to be conservative, and only support cases for which there aren't corresponding literals. E.g., `x: float = float("5")` is fine, but odd (why not use a literal?), and so why allow it?

I'm fine with either decision... Having looked at the source issue, I guess I'm leaning towards allowing arbitrary `int`, `float`, etc., calls, since they _do_ produce immutable values, and if we want to suggest that users should use `5.0` instead of `float("5.0")`, that feels like it could be the responsibility of a separate rule, if needed.


---

_Comment by @qdegraaf on 2023-05-25 21:05_

Ah nice find, thanks for the effort. Don't know why I just assumed it was a Python thing and not a bugbear design choice. Will do some more digging before throwing questions up for any future plugin port confusion. 

I agree with leaning towards allowing all immutable func calls, for the reasons you stated, as well as avoiding any similar confusion someone might have in the future. If you're happy to deviate from upstream implementation I can add some more immutable funcs, will tweak the bugbear tests and double-check both `RUF` and `B`. I can make a spinoff issue for implementing the separate rule for the `5.0` instead of `float("5.0")` etc. as well if it seems useful, 

---

_Converted to draft by @qdegraaf on 2023-05-26 18:33_

---

_@charliermarsh reviewed on 2023-05-26 18:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B008_B006_B008.py.snap`:56 on 2023-05-26 18:52_

We should probably update these comments, they're not out-of-date, I think? (Can we also remove the special-casing code in the bugbear module, which should now be redundant?)

---

_@qdegraaf reviewed on 2023-05-26 19:04_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B008_B006_B008.py.snap`:56 on 2023-05-26 19:04_

I'm not sure I follow exactly on the comments. We settled on allowing `float` calls now right? So these snapshots can go. 

I updated the comments in the `.py` fixture to reflect this. As for the Violating string it's formatted like so:

```rust
format!("Do not perform function call `{name}` in argument defaults")
```

So this needs no edit. Does that cover what you mean or are you referring to something else. 

As for the special casing float logic, I'm removing it now!

---

_@charliermarsh reviewed on 2023-05-26 19:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B008_B006_B008.py.snap`:56 on 2023-05-26 19:08_

Sorry, I meant "they're out-of-date", not "they're _not_ out-of-date". What you've done here looks correct to me.

---

_@qdegraaf reviewed on 2023-05-26 19:13_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B008_B006_B008.py.snap`:56 on 2023-05-26 19:13_

Ah check, all good! Removed infinity/nan logic now as well.

---

_Marked ready for review by @qdegraaf on 2023-05-26 19:13_

---

_Merged by @charliermarsh on 2023-05-26 19:18_

---

_Closed by @charliermarsh on 2023-05-26 19:18_

---

_Comment by @charliermarsh on 2023-05-26 19:18_

LGTM, thanks!

---
