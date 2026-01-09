---
number: 13050
title: "New simplify rule: use `sum` over `len` + `if`"
type: issue
state: open
author: janosh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-22T09:34:26Z
updated_at: 2024-08-23T10:05:47Z
url: https://github.com/astral-sh/ruff/issues/13050
synced_at: 2026-01-07T13:12:15-06:00
---

# New simplify rule: use `sum` over `len` + `if`

---

_Issue opened by @janosh on 2024-08-22 09:34_

```py
len([val for val in iterable if cond(val)]) # bad

sum(cond(val) for val in iterable) # good
```

example

```py
len([i**2 for i in range(10) if i**2 < 36])  # bad
>>> 6
sum(i**2 < 36 for i in range(10))  # good
>>> 6
```

---

_Comment by @tjkuson on 2024-08-22 18:40_

Your example suggests using the sum of a generator over the length of a list comprehension. Not sure if this was deliberate, but if this rule were to be accepted, it should probably persist iterable type given the discourse (#10838).

---

_Comment by @janosh on 2024-08-22 19:31_

That was deliberate. I would always prefer generator for brevity.

---

_Comment by @DanielYang59 on 2024-08-23 03:34_

Randomly join the conversation wonderful work Janosh, but we should also be aware of the slight performance penalty for the `sum` method.

With script:
```python
import timeit


def condition(val):
    return True if val % 2 else False


for seq_len in (10, 100, 1000, 10000, 100000):
    sequence = list(range(seq_len))

    execution_time_a = timeit.timeit(
        'len([val for val in sequence if condition(val)])',
        globals=globals(), number=1000
    )

    execution_time_b = timeit.timeit(
        'sum(condition(val) for val in sequence)',
        globals=globals(), number=1000
    )

    print(f"Run time of length {seq_len}:")
    print(f"With len if: {execution_time_a:.4f}")
    print(f"With sum: {execution_time_b:.4f}")
    print(f"Ratio: {execution_time_a / execution_time_b:.4f}\n")
```

I got (Python 3.12.5, MacOS 14.6.1, M3 chip):
```
Run time of length 10:
With len if: 0.0005
With sum: 0.0007
Ratio: 0.6783

Run time of length 100:
With len if: 0.0042
With sum: 0.0054
Ratio: 0.7937

Run time of length 1000:
With len if: 0.0321
With sum: 0.0345
Ratio: 0.9320

Run time of length 10000:
With len if: 0.2593
With sum: 0.3485
Ratio: 0.7441

Run time of length 100000:
With len if: 2.6005
With sum: 3.4534
Ratio: 0.7530
```

---

_Label `rule` added by @dhruvmanila on 2024-08-23 04:23_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-23 04:23_

---

_Comment by @tjkuson on 2024-08-23 09:52_

Running the same script on Python 3.12 with a sum of a list comprehension (`sum([condition(val) for val in sequence)]`) narrows the difference and is sometimes faster.

```
Run time of length 10:
With len if: 0.0006
With sum: 0.0007
Ratio: 0.8689

Run time of length 100:
With len if: 0.0048
With sum: 0.0052
Ratio: 0.9311

Run time of length 1000:
With len if: 0.0380
With sum: 0.0341
Ratio: 1.1151

Run time of length 10000:
With len if: 0.3036
With sum: 0.3147
Ratio: 0.9647

Run time of length 100000:
With len if: 3.0352
With sum: 3.1247
Ratio: 0.9713
```

On 3.9,

```
Run time of length 10:
With len if: 0.0008
With sum: 0.0008
Ratio: 0.9064

Run time of length 100:
With len if: 0.0062
With sum: 0.0067
Ratio: 0.9225

Run time of length 1000:
With len if: 0.0456
With sum: 0.0442
Ratio: 1.0327

Run time of length 10000:
With len if: 0.3862
With sum: 0.4296
Ratio: 0.8991

Run time of length 100000:
With len if: 3.8578
With sum: 4.2645
Ratio: 0.9047
```

---

_Comment by @DanielYang59 on 2024-08-23 10:05_

Thanks a lot for the input @tjkuson. Interestingly my previous results were also generated with Python 3.12 on MacOS 14.6.1 with M3 chip (sorry I forgot to mention these details, would update the results).

---
