```yaml
number: 8378
title: B008 complains about function call when constructing a default parameter value as a exception
type: issue
state: closed
author: RonnyPfannschmidt
labels:
  - needs-decision
assignees: []
created_at: 2023-10-31T13:09:24Z
updated_at: 2023-11-07T04:53:47Z
url: https://github.com/astral-sh/ruff/issues/8378
synced_at: 2026-01-10T11:09:50Z
```

# B008 complains about function call when constructing a default parameter value as a exception

---

_Issue opened by @RonnyPfannschmidt on 2023-10-31 13:09_

https://play.ruff.rs/54edf8a4-4c2f-4c0e-a5d8-b7b4afa7a19e

a definition like 
```py
def pain( error: Exception | None = ValueError("Hosts weren't successfully added")):
    pass
```

triggers
```
B008: Do not perform function call `ValueError` in argument defaults`
```

---

_Comment by @charliermarsh on 2023-10-31 17:00_

I'm definitely open-minded about improving the rule, but can you say a bit more about why you think this case should be excluded? Alternatively, why prefer this over the in-body initialization pattern?

```python
if error is None:
    error = ValueError("Hosts weren't successfully added")
```

---

_Label `waiting-on-author` added by @charliermarsh on 2023-10-31 17:00_

---

_Comment by @RonnyPfannschmidt on 2023-10-31 19:16_

In my case none is a valid meaningful parameter variant

Creating sentinels as replacement is pretty painful 

---

_Comment by @charliermarsh on 2023-10-31 20:12_

Ahh I see, so passing in `error=None` has meaningfully different behavior. And I assume you don't want to define the sentinel in the module body.

The challenge here is that I don't know how to avoid this without disabling the rule altogether. Even here, `error` isn't immutable, so calls to `pain` could modify the shared error value...

---

_Comment by @RonnyPfannschmidt on 2023-10-31 20:32_

The rule is about function calls,I believe it's fair game to allow construction of built-in exceptions 

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-10-31 23:31_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-31 23:31_

---

_Comment by @RonnyPfannschmidt on 2023-11-05 21:54_

https://github.com/PyCQA/flake8-bugbear/blob/907e0dd29a99818591a604d4557c70ea33204712/bugbear.py#L1660-L1667 has more details in the error

https://github.com/astral-sh/ruff/blob/4170ef050853998f2ea3423e7fbf0a5c38a9323b/crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs#L60 is rather barebones

i would recommend to extend the error with the suggestion

---

_Comment by @charliermarsh on 2023-11-05 22:09_

Does that suggestion solve the problem for you though? Do you consider that a reasonable workaround?

---

_Comment by @RonnyPfannschmidt on 2023-11-05 23:52_

Yes 

---

_Closed by @charliermarsh on 2023-11-07 04:53_

---
