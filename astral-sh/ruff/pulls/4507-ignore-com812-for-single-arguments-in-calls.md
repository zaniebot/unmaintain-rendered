```yaml
number: 4507
title: Ignore COM812 for single arguments in calls
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: ignore-com812-for-single-arguments-in-calls
created_at: 2023-05-18T19:40:07Z
updated_at: 2023-06-27T22:45:50Z
url: https://github.com/astral-sh/ruff/pull/4507
synced_at: 2026-01-12T15:55:15Z
```

# Ignore COM812 for single arguments in calls

---

_@JonathanPlasse_

- Discussed in #3303

Black does not add a comma in this case.
In most cases, the call will be added other arguments so the comma adds noise and is not natural looking.

---

_Comment by @github-actions[bot] on 2023-05-18 19:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.0±0.05ms     2.7 MB/sec    1.00     14.9±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    378.2±7.72µs     7.8 MB/sec    1.00    376.7±1.83µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1545.7±6.34µs    10.8 MB/sec    1.01   1556.6±5.17µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.0±1.53µs    17.8 MB/sec    1.02    169.4±0.28µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.00ms     7.6 MB/sec
parser/large/dataset.py                    1.00      6.0±0.00ms     6.8 MB/sec    1.14      6.9±0.01ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1175.5±2.11µs    14.2 MB/sec    1.12   1312.1±0.82µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    121.7±0.25µs    24.2 MB/sec    1.09    132.3±0.60µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.00ms    10.0 MB/sec    1.11      2.8±0.00ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.4±0.14ms     2.3 MB/sec    1.00     17.2±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.04ms     3.7 MB/sec    1.00      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.1±5.88µs     6.5 MB/sec    1.00    453.1±7.18µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.07ms     3.5 MB/sec    1.00      7.3±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.05ms     4.7 MB/sec    1.00      8.6±0.06ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1808.9±12.40µs     9.2 MB/sec    1.00  1800.6±13.13µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.8±1.97µs    15.3 MB/sec    1.01    194.8±8.66µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.02ms     6.5 MB/sec    1.00      3.9±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      7.1±0.03ms     5.8 MB/sec    1.00      7.1±0.03ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1334.4±9.76µs    12.5 MB/sec    1.00  1337.5±11.64µs    12.4 MB/sec
parser/numpy/globals.py                    1.01    138.5±1.84µs    21.3 MB/sec    1.00    137.7±1.33µs    21.4 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.02ms     8.5 MB/sec    1.00      3.0±0.02ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-19 02:05_

It seems like the originating plugin has different behavior -- it flags for trailing comma as soon as the arguments span multiple lines. So it allows `f(a, b)`, but not:

```py
f(
  a, b
)
```

What do you think about that heuristic?

---

_Comment by @JonathanPlasse on 2023-05-19 06:52_

I agree with you mostly.
I would like to avoid adding a comma for a long single argument like in the code below.
Recently I changed the line-length from 88 to 100 and if COM812 did not add a comma it could have fit on a single line.
```diff
 class Foo:
     def get_land_frame_delta(self, drone_hgt_meter: float) -> int:
         return FRAME_PARAMETERS.from_second_to_frame(
-             self.get_land_second_delta(drone_hgt_meter),
+             self.get_land_second_delta(drone_hgt_meter)
         )
```

---

_Comment by @bersbersbers on 2023-05-24 11:49_

> trailing comma as soon as the arguments span multiple lines

I agree with this heuristic. I have a number of lines like this which are flagged
```python
def long_function_name_that_is_really_very_long(*args):
    ...

long_function_name_that_is_really_very_long(
    9999999999999999999999, 9999999999999999999999
)
```
and when `--fix`ed, broken into several lines by black.

---

_Closed by @charliermarsh on 2023-06-02 15:33_

---

_Branch deleted on 2023-06-02 16:04_

---

_Comment by @bersbersbers on 2023-06-27 22:45_

Since this as well as #3303 has been closed, is there a follow-up issue or PR that I can track for this? (I would assume that being incompatible with `black` is something that will be tackled at one point.)

---
