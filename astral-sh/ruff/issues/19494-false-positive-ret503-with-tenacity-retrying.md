```yaml
number: 19494
title: "False positive `RET503` with `tenacity.Retrying`"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-22T18:51:44Z
updated_at: 2025-07-24T10:05:29Z
url: https://github.com/astral-sh/ruff/issues/19494
synced_at: 2026-01-10T11:09:59Z
```

# False positive `RET503` with `tenacity.Retrying`

---

_Issue opened by @jamesbraza on 2025-07-22 18:51_

### Summary

The below Python code with Python 3.13.5 and `tenacity==9.1.2`:

```python
import random

from tenacity import Retrying, retry_if_exception_type, stop_after_attempt


def try_lottery() -> float:
    for attempt in Retrying(
        retry=retry_if_exception_type(RuntimeError), stop=stop_after_attempt(3)
    ):
        with attempt:
            if random.random() < 0.001:  # Some stochastic condition
                return 1e6
            raise RuntimeError("The lottery was not won.")
```

The presence of the `return 1e6` activates `RET503` for beyond the `for` loop. However, the code after the `for` loop is not actually reachable, due to the internal implementation details of `tenacity.Retrying`.

This is a deep `RET503` false positive that I am not sure Ruff can do anything about, but I figured I would report nonetheless.

Note, this would apply to both `tenacity.Retrying` and `tenacity.AsyncRetrying`.

### Version

0.12.4 (ee2759b36 2025-07-17)

---

_Comment by @ntBre on 2025-07-22 23:08_

Thanks for the report! That does seem tricky to do much about, even with type inference. I took a look at [`Retrying.__iter__` ](https://github.com/jd/tenacity/blob/eed7d785e667df145c0e3eeddff59af64e4e860d/tenacity/__init__.py#L436) and it just returns a `typing.Generator`, which I don't think says anything about its length (assuming that an empty iterable is the only way this function can return implicitly). The only other workarounds I can think of would be adding a special case or a config option.

On the other hand, we don't currently inspect the body of the loop at all:

https://github.com/astral-sh/ruff/blob/dc10ab81bd85b8af49ee2fe8ed4a6f767d37d6e0/crates/ruff_linter/src/rules/flake8_return/rules/function.rs#L505-L511

We could possibly err more on the side of false negatives if we assume that the loop is entered. Without the loop, your example doesn't trigger RET503:  https://play.ruff.rs/19c58c03-7ccf-4e1c-8b48-8a6275d35469.

---

_Label `rule` added by @ntBre on 2025-07-22 23:08_

---

_Label `needs-decision` added by @ntBre on 2025-07-22 23:08_

---

_Comment by @MichaReiser on 2025-07-23 06:44_

> We could possibly err more on the side of false negatives if we assume that the loop is entered. 

Can you say more how you would want to change the rule. We still want it to flag

```
def try_lottery(items) -> float:
    for attempt in items:
        with attempt:
            if random.random() < 0.001:  # Some stochastic condition
                return 1e6
            raise RuntimeError("The lottery was not won.")
```


which isn't such an uncommon error that someone forgets to handle the case where `items` is empty.

I don't think there's anything Ruff can reasonably do here other than special casing which I'm not convinced is worth it here (we would need to detect that it iterates over a `Retrying` and all code is within the `with attempt` block. There's also the case where it throws an exception not handled by the attempt, but that's explicitly handled with a try...catch, in which case ret would apply again. 

---

_Comment by @ntBre on 2025-07-23 12:26_

I didn't have anything concrete in mind, it was just a half-baked idea after seeing the implementation. I agree with your assessment.

---

_Comment by @MichaReiser on 2025-07-24 10:05_

I'll close this as out of scope for ruff at the moment and I want to keep the issue tracker actionable. We might revisit this once ruff uses ty's semantic model.

---

_Closed by @MichaReiser on 2025-07-24 10:05_

---
