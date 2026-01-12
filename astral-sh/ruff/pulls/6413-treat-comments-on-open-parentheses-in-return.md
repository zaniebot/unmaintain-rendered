```yaml
number: 6413
title: Treat comments on open parentheses in return annotations as dangling
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/dangling
created_at: 2023-08-08T01:55:07Z
updated_at: 2023-08-08T20:48:40Z
url: https://github.com/astral-sh/ruff/pull/6413
synced_at: 2026-01-12T15:55:21Z
```

# Treat comments on open parentheses in return annotations as dangling

---

_@charliermarsh_

## Summary

Given:

```python
def double(a: int) -> ( # Hello
    int
):
    return 2*a
```

We currently treat `# Hello` as a trailing comment on the parameters (`(a: int)`). This PR adds a placement method to instead treat it as a dangling comment on the function definition itself, so that it gets formatted at the end of the definition, like:

```python
def double(a: int) -> int:  # Hello
    return 2*a
```

The formatting in this case is unchanged, but it's incorrect IMO for that to be a trailing comment on the parameters, and that placement leads to an instability after changing the grouping in #6410.

Fixing this led to a _different_ instability related to tuple return type annotations, like:

```python
def zrevrangebylex(self, name: _Key, max: _Value, min: _Value, start: int | None = None, num: int | None = None) -> (  # type: ignore[override]
):
    ...
```

(This is a real example.)

To fix, I had to special-case tuples in that spot, though I'm not certain that's correct.


---

_Review requested from @konstin by @charliermarsh on 2023-08-08 01:55_

---

_Label `formatter` added by @charliermarsh on 2023-08-08 01:55_

---

_Review request for @konstin removed by @charliermarsh on 2023-08-08 02:01_

---

_Converted to draft by @charliermarsh on 2023-08-08 02:01_

---

_Marked ready for review by @charliermarsh on 2023-08-08 02:34_

---

_Converted to draft by @charliermarsh on 2023-08-08 02:55_

---

_Comment by @charliermarsh on 2023-08-08 02:55_

(Please do not spend time reviewing this, intentionally in draft.)

---

_Comment by @github-actions[bot] on 2023-08-08 03:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.2±0.16ms     5.0 MB/sec    1.00      8.1±0.11ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.02  1632.8±43.36µs    10.2 MB/sec    1.00  1593.7±23.59µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    178.9±5.30µs    16.5 MB/sec    1.00    178.2±6.51µs    16.6 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.10ms     7.3 MB/sec    1.00      3.4±0.07ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.05ms     4.0 MB/sec    1.00     10.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.2 MB/sec    1.00      2.7±0.02ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    380.5±2.36µs     7.8 MB/sec    1.00    378.3±1.21µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.7±0.03ms     5.5 MB/sec    1.00      4.7±0.04ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1121.1±4.19µs    14.9 MB/sec    1.01   1134.0±3.83µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    130.4±6.42µs    22.6 MB/sec    1.00    126.8±2.62µs    23.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.01ms    10.9 MB/sec    1.01      2.4±0.03ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.11ms     4.1 MB/sec    1.01     10.0±0.15ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1868.6±22.95µs     8.9 MB/sec    1.02  1909.2±34.52µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    200.1±3.84µs    14.7 MB/sec    1.07    213.2±6.04µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.02      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.7±0.16ms     3.2 MB/sec    1.00     12.6±0.15ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.9±5.43µs     7.0 MB/sec    1.01    425.9±7.30µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.10ms     4.4 MB/sec    1.00      5.8±0.06ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.08ms     6.0 MB/sec    1.00      6.8±0.06ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1399.4±17.10µs    11.9 MB/sec    1.00  1394.8±14.71µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.4±3.29µs    18.5 MB/sec    1.01    161.1±3.27µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.6 MB/sec    1.01      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-08-08 06:45_

I think we need a more holistic approach to the trailing open parentheses comments problem. Solving the placement node by node doesn't scale because the problem exists for every expression:

```python
a = a + b + c + d + ( # Hello
    e + f + g
)
```

I suggest that we motivate why we want to keep the comments on the same line as the opening parentheses (prettier and rustfmt don't)  and then propose possible implementations (this can range from one-off fixes, introducing parenthesized expressions or something else)

---

_Comment by @charliermarsh on 2023-08-08 11:47_

I agree, and I will work on that.

Note though that the goal in this PR isn’t even to explicitly keep the comment on the same line (whether it gets formatted on the same line or it’s own new line is orthogonal), it’s to ensure it’s associated the correct node. At present, it’s associated with the wrong node, and only works “by accident”.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:797 on 2023-08-08 12:31_

Could you the example above and
```python
def double(a: int # Hello
) -> (int):
    return 2 * a
```
to the test cases?

---

_@konstin reviewed on 2023-08-08 12:32_

`handle_leading_returns_comment` looks correct

---

_Comment by @charliermarsh on 2023-08-08 13:17_

I will write up a more thorough analysis with some ideas on how to handle in general, but in the meantime I've collated examples for every relevant AST node [here](https://play.ruff.rs/?secondary=Format#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsovYAMleQbC1QW5ooPPIDxJx4upAADtIAzABGFwxXL4WDQADWKFWGGguHSWz4eEEYFAEFEMj+GH4nFemR4hEGunwcGKsVExEIPx+mWIxAw4iksiUGlRpNg5IuUCpNLpDPSuFmBKJuH4GhJZJQFKgcEQSCKTzhmjhuGkks50u5kDGGVmRHgsg6PGkP3QGuKbAAvr1wVCYRgyPC0HjdOiMZAlko0Dg0OgMDwJD9GZxMmUMurzhiPQB6QM-UHR0QxhSKONBxNJyAxv1yGPEJYSO7Rh6iLBLFBYSEB03++JYFA-ZG4C0oa22iHQ2EcDRkFBKN3atopKSELYUWZYOWopjtsGdh0cbh8ASD6OQJe8fgM3WZabEM4MRhz0R2rvYBBBzIVgSfVd0d2iOXIENlbDTjBIPFLDDQ6RIDJMFZSQkh4dkpRPKAzwdHA-mUDQlkPR8oERTgG3UbRvgMEJODCABhBA4NSRCAB0SOIEwDDIgw8LIsIyIosIQjItBgAOK0DAwFi2KtMJvEzTF+AwOo1A+PZdBcSDIGg2EBkUCRiAlB9tQkaAtEVZQFjmJJFhWOhJIgG153tWEAEdCAQRJUWQnVcGmIUMHMyyqG+NAEEICcBKgCRCC3ezZicqzXPczyZTEBAsAPTSNEciygrocKPMoLyYDkBBOEwKgpzNXQeEUQg20MjsTKKEp8DXD1d1mIYJBcqMsyMVToS80QjGgYgXTKFqoCMTIfghBtusgIw6hKNBqBlVrRtgNAjD9UlfLAoajBqlAjGWhRYAK9buWPIrjPPPg0FkTggwyJblPXJ1cHUEpoD4ORZlOn5ztRX4lFAibtWuoUgJ+Tg3StKSZJrM1sArKtxQqyk8rQw8OS5dd4hQINXgkBBxEoGduVEE1-oWYsMVLKB8twPhaowFBNsIe7-SdYpsAhek6qPMLSfJ2YqbgGnEkwP0cEUe6MhnPawCM08F1hDQXkSBJoagjoyjvDBVuxpMeqG6WeFl8pJpQrWdcJiBib6ApakV5sVegWqReByWcSDaYsDxDSL1wKckO1Z8FR8vyZly-LCrF4rz2e863cesm72sr3pnalnkNEH5nXa74fjQIa-iURRkDT6BM8QHhYGkAA6eJaSoN6fh0PXIGKcgMhbBLiFXWuCmIDJwVzhKeHK2vcEkP5vlwBNa9Unh+ss6YyG+R2hvHyei84MgS7+Re094IaeEhfhEkUb5t8zhAIUUKvYEz+ESjTlta7gLZQ2+O+hqWY+EDkTgUCQN6ljkI2xbtkqfwrKvAPNIZKl0PR1ASESWYydMhkwrMUVEeUCphTgdbF4yg9hWxtqDfYkBCT9SyGgpQGDYbYOplQPB3x9y621Og2q5DZiUIZDnBUeN8GEOSmzaAnAkG3XMqGWY49yzCQyNkHGUAABC8cACiWgGxNjvENeRijmxDQAGrcxQLIxQOd961wAPIAGVdH6KGgASUMWY8RtdZG4HfjnXAtUyY2IMZIuuEVoQ8DLno8Ru0eF8KocOARXBMgYBET+fA4jmhhVUooascDQKIJZigoO4soL2w6CQf0wEXEXTRNqKq8glAqE6jglmCN0m9D4edeW0kMjoSQILYMcAVBINbGFaJih0IfAWGofynTtR9I0AMmYlMFFbQ+CiSYHocASDIOM9qDsXqKAKVUsKxB+ouzwAGQW3BxQXikPnOgaSwqAQGI6WQHCA6oO1N03p4cvzLCEsUJWMy6AbO1FOBmxRbwPX2F89cDzZg8AQAGQiszRCQnSEgG6dRT6vDgSaKFUAYXIBui8xQmBkWRlZtqdFcK5gRTgGImaAxUWm0FoqFO2K5jL0FooPFOR1x7nupwR6KzXoYAufvBKRA1mINeGCxmCAkF0NZSgQRmQRyrIKSyuZTMOmxO+SiLYZMRZhVTCoCcmrtTpHpUKBkfZun7AVVMGYDJoD4D3lytZqIMCAjCv5Y1LxUBJDwaiFwXTGn+mKOgxIlK3mW15RI9W0kxwwKGgeWlmBp6MvVG3JYoYcVKBRbXBFB4MC4qGogKcsAyV+n3gE4ZlZmzWSBvtTEWApx9nqapdS8yiFaDxHi4EUkzQ-FhDVKGEDRDFNVmGrMxQeAAFVR4eMSEoAAIhi6NLxx14SVVvfsihZ1wqXe1L64aR3joALIY18sQydq7124APZjY94b2rSHdsYhdE7r3EFvVgAAKqeudtdd0-HfQead9184Zt4VtTIqjGzqNrogBYe7K7QGlvPNS07OD4HKiW9cWAlWMJfnzSsGQhanyDUiNCWGMaMlw4LMFBHmgdukL6KgJpwGFPXMUt+AwoMxS2MQas8z8ngU1DRtykUGP7BssU-meHKN6vXLSQiAwTRkYFvh22VaoB-GgjHJG-RCjSyFMoSKFQaP2U9uub2ES4NoRKVtUFtY1bDrykNMg0ggpoY9A2x0gt3aIN0ICAATHEtSt1CSDFRAANn8+pVkqIACs4WaWJF47oKLBlg65D+NSDQgs-T1OhI2dQxpTrWfxkpMAVTrQgCtJESrABiGrIAqtgAAApio+BQWQAsww8Dq7VurYBX3JuIGAPhUB2tlsgGAAwlBoAugWGAHjoEwB4DAAAAy0EtsIJcesAAlkBUwGMwMAwFVDSH28sFAs3Lz5LAAWdyM0wB9jAJQG1C3cBgFOz10gtUHszAOwN07y2kDJsoEtjbAPuBnYMGAerc2ybci0CAMINBuTJ3pCAHrfWqBnY+sNjIHWxsTf7NNjQ52pCfWe8t1b62evGIwbN6Aj17o-YO2QVjG3kPjch8T-JsP4eI+jMj4gIAShs4h1Di7oFucI6R1u1H9W+tDaG5AEbXVxuTcJ5z+bi2VtrY2-V6nn2pz054Izhuj0QffnZ6LknMPoxw8l3z6XaP+uDYG4rnHo2VcE-FOrsmZOltaKs247XVOacG-7Eb9qTOWcgDytIXnGJ+eC4UeB8bluufRn9wVNxPOpco8d-Ll3Svyge6m176HRvNcU512APXZ3Q8M4jyblAG2RSDAt97zrNv4cy9607hXhe8eq9L2Ln3Feg+65D3TsPxvmem5AH6WAbey8S+73Lgbfe3fK-xyXmbZffeV+D-ryf9eBuN+b7w4oi-h8d4xLblfveC8b6LwYNA8JRlD6t+Xl7f2Y3xdApT-AOcEgYAWgg2Z0ayl+H+3I0gXeeea+D+ei7uz+r+9kROu+i2f2eSn0lOGGW6YAAAYggAgCLu3tyDIooNnvbrnrLvftjggZvi-v0u-pdugRWAdpoCBBqv-oQcQUvp3mEDLrVvVkuqusXINu7I-l1lVnfvnq9vsige3r7gQQgEtvthwEbtdr5GgHdpjg9p7jNhkGAAwW-gsD1mgS9ktmQcDiAFoHQEoWAAALwQFp4YhkEwHUEyF5TAZMEa7mGraqFjhXYvyaHaEHa6Hb5E4GFGEoGmFX6+7SBWEgEmCOE8FX5QFuE94eFyHeEj6+EqF3YBEaG3b3bQBhFq6RHIHigxEf5xFWHdJOHi58FiH1HW4YjQF27x4O7uFwGhGeEHI76xGj7+HqFBFFE6GD76GKCGEVEmGp4+HLbxEbbzQlYGBaBhBgAYAAB8zR1+EAbRceEACesBzuPRWR-R1Rgx+RwxN2WhxRpRXu5RjBMxChmuCxIAW6cmFuEu+2KRkB0YbRIAQAA).

---

_Marked ready for review by @charliermarsh on 2023-08-08 19:20_

---

_Review requested from @konstin by @charliermarsh on 2023-08-08 19:24_

---

_@konstin approved on 2023-08-08 20:20_

---

_Merged by @charliermarsh on 2023-08-08 20:48_

---

_Closed by @charliermarsh on 2023-08-08 20:48_

---

_Branch deleted on 2023-08-08 20:48_

---
