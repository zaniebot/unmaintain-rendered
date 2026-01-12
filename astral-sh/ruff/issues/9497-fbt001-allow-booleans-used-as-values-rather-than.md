```yaml
number: 9497
title: "FBT001: Allow booleans used as values rather than flags"
type: issue
state: open
author: arvidfm
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-01-12T21:07:38Z
updated_at: 2024-05-06T14:59:09Z
url: https://github.com/astral-sh/ruff/issues/9497
synced_at: 2026-01-12T15:54:49Z
```

# FBT001: Allow booleans used as values rather than flags

---

_@arvidfm_

The post that originally inspired the FBT rules is concerned specifically with boolean flags that modify the behaviour of a function, and in that context they make a lot of sense in terms of readability, and I like to have the rules enabled for that reason. However, there are cases where the boolean _is the value_ as opposed to a flag. You might encounter this for instance when dealing with functions for enabling options, where the meaning of the option is given by the function or object name itself, not the parameter: `config.enable_fallback(False)`, `self.is_running.set_value(True)`.

I think the linter already recognises this to some extent, as it already has exceptions for some functions that fall into this exact category (`set_blocking`, `set_enabled`, `__setitem__` - albeit the first two for calls rather than definitions), but rather than adding individual exceptions, I think the rule could be relaxed in some clear-cut cases.

The most obvious case that I can think of where a bool is a value rather than a behaviour-modifying flag is when a function only takes a single positional-only parameter:

```py
def enable_secret_mode(value: bool, /) -> None: # allow
    ...

class A:
    def set_running(self, value: bool, /) -> None: # allow - don't count self
        ...
```

One could also consider allowing defining functions that have multiple arguments, as long as the only positional(-only?) bool is the first argument. Or perhaps functions that only take positional-only arguments, and at most one of them is a boolean (think `os.set_blocking`).

This would also help when defining callbacks, where you can't use keyword-only parameters:

```py
# can't use keyword-only parameter for callbacks
# no way to make the linter happy here
def my_callback(ok: bool, /) -> None:
    ....

# function takes a Callable[[bool], None]
do_thing("value", callback=my_callback)
```

---

_Label `question` added by @charliermarsh on 2024-01-13 00:51_

---

_Label `rule` added by @charliermarsh on 2024-01-13 00:52_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-13 00:52_

---

_Label `question` removed by @charliermarsh on 2024-01-13 00:52_

---

_Comment by @Rizhiy on 2024-03-02 22:29_

I think `FBT001` & `FBT003` should not trigger in general if it is the only positional argument, or perhaps if it is the first.
In most cases, the intent should be obvious from the function name, similar to cases above.

---

_Comment by @Avasam on 2024-03-09 18:06_

In my case I got a false-positive for a method that takes more than 1 parameter, but there's only a single boolean param and it's called "value" in a method that starts with `set_`. Some heuristic with standard names in those cases might help. (but it's also my only false-positive, and it's obvious I'm not abusing `noqa` in that case).
![image](https://github.com/astral-sh/ruff/assets/1350584/502a8282-f3a8-4f48-bd15-f00cad0edfc2)


---

_Comment by @johnslavik on 2024-04-16 22:57_

+1 on this.
I often use flag-holding context vars.
```py
from contextvars import ContextVar

cv = ContextVar("cv", default=False)
cv.set(True)  # triggers FBT003
```

---

_Comment by @charliermarsh on 2024-05-06 14:59_

`cv.set(True)` is now ignored.

---
