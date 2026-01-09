---
number: 15908
title: "Should Ruff have an opinion on math.inf vs float(\"inf\")?"
type: issue
state: open
author: RubenVanEldik
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-02-03T11:55:29Z
updated_at: 2025-02-04T10:49:15Z
url: https://github.com/astral-sh/ruff/issues/15908
synced_at: 2026-01-07T13:12:16-06:00
---

# Should Ruff have an opinion on math.inf vs float("inf")?

---

_Issue opened by @RubenVanEldik on 2025-02-03 11:55_

### Description

Today I found out that there are two ways to define an infinity floating variable in Python:

1. `math.inf`
2. `float("inf")`

After quick testing it seems that they produce exactly the same object, but `math.inf` is 2-3 times faster. On top of that `math.inf` is shorter than `float("inf")`. Lastly, I think `math.inf` looks more pythonic than `float("inf")`, but that might be more personal.

Should we add a Ruff rule to prefer `math.inf` over `float("inf")` (and `-math.inf` over `float("-inf")`)

Keywords:
- math.inf
- float("inf")
- infinity

---

_Comment by @AlexWaygood on 2025-02-03 12:25_

> After quick testing it seems that they produce exactly the same object, but `math.inf` is 2-3 times faster.

Which Python version did you test this on, out of interest? A lot of this may just be the overhead of loading and calling a builtin symbol, which has got quite a bit faster in recent versions. (And it may continue to get faster!)

---

_Label `rule` added by @AlexWaygood on 2025-02-03 12:25_

---

_Label `needs-decision` added by @AlexWaygood on 2025-02-03 12:25_

---

_Comment by @RubenVanEldik on 2025-02-03 13:11_

I tested this on Python 3.12.

I currently don't have 3.13 installed on my computer (sorry, I'm not using uv yet haha.... 😬😬😬)

---

_Comment by @AlexWaygood on 2025-02-03 13:15_

Hehe. 3.12 is pretty new; that's good enough for me.

Overall this seems like a reasonable stylistic lint to me. We're currently deprioritising adding new rules, however, especially stylistic rules (we'd prefer to maintain and improve what we already have for a little bit)

---

_Comment by @MichaReiser on 2025-02-03 13:20_

Another data point: Type checkers can easily catch `math.in` as a misspelling of `math.inf` but tools are probably less good at identifying `float("in")` (which is also a less obvious API)

I do agree that this is more a stylistic lint than a performance lint.

---

_Comment by @RubenVanEldik on 2025-02-03 13:23_

I was curious, so I quickly ran the tests. 

It seems that 3.9 to 3.11 added massive improvements, but that the improvements from 3.11 onward are more modest. 

Interestingly, the relative difference between the two seems to have grown from about 2x to 4x!

```py
import math
import timeit

time_math_inf = timeit.timeit('math.inf', globals=globals(), number=10**8)
print(f'math.inf: {time_math_inf:.2f} seconds')

import timeit
time_inf_str = timeit.timeit('float("inf")', number=10**8)
print(f'float("inf"): {time_inf_str:.2f} seconds')
```

Python 3.9
```
math.inf: 2.72 seconds
float("inf"): 5.80 seconds
```

Python 3.10
```
math.inf: 1.80 seconds
float("inf"): 3.56 seconds
```

Python 3.11
```
math.inf: 0.73 seconds
float("inf"): 2.81 seconds
```

Python 3.12:
```
math.inf: 0.8 seconds
float("inf"): 2.82 seconds
```

Python 3.13
```
math.inf: 0.70 seconds
float("inf"): 2.89 seconds
```

---

_Comment by @RubenVanEldik on 2025-02-03 13:26_

I understand that this currently does not have the highest priority. (personally I hope you're spending all you energy on red-knot, because I can't wait to ditch Mypy) I'm not sure if I have the time, but would you be open to a PR from the community for this rule? Or do you want to reduce the number of new rules to Ruff altogether?

---

_Comment by @MichaReiser on 2025-02-04 10:49_

We're trying to hold off from adding new "restriction" or stylistic rules until we've completed https://github.com/astral-sh/ruff/issues/1774 because it will make it easier for users to select the right rules vs today where `RUF` is a wild mix of correctness, stylistic, restriction etc. rules. 

Another reason why we're more strict about adding more rules is that we want to focus on type checking and working on core Ruff features (the mentioned rule categorization, unified format and check command, better diagnostics), and reviewing and maintaining new rules does reduce the time we can spend on those. But we also don't want to move away excited contributors. I'd be open to review a PR if your excited about adding the rule now.

---

_Referenced in [pylint-dev/pylint#10621](../../pylint-dev/pylint/pulls/10621.md) on 2025-10-06 11:12_

---
