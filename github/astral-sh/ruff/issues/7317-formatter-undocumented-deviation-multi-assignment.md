---
number: 7317
title: "Formatter undocumented deviation: multi assignment wrapping"
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-12T20:53:26Z
updated_at: 2023-09-29T17:27:34Z
url: https://github.com/astral-sh/ruff/issues/7317
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter undocumented deviation: multi assignment wrapping

---

_Issue opened by @JonathanPlasse on 2023-09-12 20:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
def from_user_altitude_range_to_px4_altitude_range(
    altitude_range: Tuple[float, float],
) -> Tuple[int, int]:
    user_minimal_coordinate, user_maximal_coordinate = (0.0, 0.0, altitude_range[0]), (
        0.0,
        0.0,
        altitude_range[1],
    )
```
Ruff formatting
```python
def from_user_altitude_range_to_px4_altitude_range(
    altitude_range: Tuple[float, float],
) -> Tuple[int, int]:
    user_minimal_coordinate, user_maximal_coordinate = (
        (0.0, 0.0, altitude_range[0]),
        (
            0.0,
            0.0,
            altitude_range[1],
        ),
    )
```
Use Ruff 0.0.289 with line length 100.

---

_Comment by @zanieb on 2023-09-12 20:57_

For what it's worth, I think the Ruff formatting is more readable in this example.

---

_Comment by @JonathanPlasse on 2023-09-12 21:01_

I am reporting all deviations I come across that are not documented.
I agree with you Ruff formatting is more readable to me.

---

_Comment by @charliermarsh on 2023-09-12 21:05_

Thanks @JonathanPlasse! That’s great and the intended workflow. We’ll collect undocumented deviations, then make decisions on whether to address them or not.

---

_Label `formatter` added by @charliermarsh on 2023-09-12 21:05_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-12 21:05_

---

_Comment by @MichaReiser on 2023-09-13 06:37_

Thanks for reporting this deviation. This is an intentional deviation that I'm open to discussing and that we forgot to document. 

Ruff parenthesizes tuples except in a few places, whereas Black strips the parentheses in many places. 

```python
# Black
def from_user_altitude_range_to_px4_altitude_range():
    user_minimal_coordinate, user_maximal_coordinate = (0.0, 0.0, altitude_range[0]), (
        0.0,
        0.0,
        altitude_range[1],
    )

    (0.0, 0.0, altitude_range[0]), (
        0.0,
        0.0,
        altitude_range[1],
    )

    assertEquals(0.0, 0.0, altitude_range[0]),

# Ruff
def from_user_altitude_range_to_px4_altitude_range():
    user_minimal_coordinate, user_maximal_coordinate = (
        (0.0, 0.0, altitude_range[0]),
        (
            0.0,
            0.0,
            altitude_range[1],
        ),
    )

    (
        (0.0, 0.0, altitude_range[0]),
        (
            0.0,
            0.0,
            altitude_range[1],
        ),
    )

    (assertEquals(0.0, 0.0, altitude_range[0]),)

```

Our reasoning behind requiring the parentheses it otherwise can be very hard to spot that it is a tuple. For example, would you have noticed that the `assertEquals(0.0, 0.0, altitude_range[0]),` is a "useless" tuple? I did not and neither did some contributors to the django project https://github.com/django/django/pull/17181. That's why I propose keeping Ruff's formatting but I'm interested in getting more feedback on this (ideally from other projects trying ruff, are there places where this formatting looks worse?)



---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 06:37_

---

_Referenced in [astral-sh/ruff#7053](../../astral-sh/ruff/issues/7053.md) on 2023-09-13 07:40_

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 15:06_

---

_Comment by @charliermarsh on 2023-09-27 15:09_

We're considering this an intentional deviation. Can be closed once it's documented.

---

_Label `documentation` added by @MichaReiser on 2023-09-27 16:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:45_

---

_Referenced in [astral-sh/ruff#7679](../../astral-sh/ruff/pulls/7679.md) on 2023-09-27 20:18_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---
