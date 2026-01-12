```yaml
number: 10409
title: ARG002 on callbacks
type: issue
state: closed
author: matthiasverstraete
labels:
  - question
assignees: []
created_at: 2024-03-14T07:40:14Z
updated_at: 2025-09-11T16:43:21Z
url: https://github.com/astral-sh/ruff/issues/10409
synced_at: 2026-01-12T15:54:50Z
```

# ARG002 on callbacks

---

_@matthiasverstraete_

Hi,

I'm not sure if this is the right place to post this question, so please redirect me if necessary.
Below is a code example:

some_library.py
```python
def foo(data: List[int], callback: Callable[[str, int], int]) -> None:
    total = 0
    for d in data:
        total += callback(str(d), d)
```

my_code.py
```python
from some_library import foo

def my_callback(name: str, value: int) -> int:
    return value * 2

foo([1,2,3,4], my_callback)
```

In this example, the rule ARG002 is triggered because I'm not using `name` in my callback. However, I can't remove that argument or mypy will complain that the type doesn't match.

In this situation, is the only solution to add `# noqa: ARG002`, or is there a proper way to handle this?

Thanks for your input!

---

_Comment by @charliermarsh on 2024-03-14 15:43_

You could change `name: str` to `_name: str`, but that also carries some risk, since it will break for users that pass keyword arguments. In general, yeah, I think a `# noqa` is required here if you're trying to adhere to some informal but common interface.

---

_Label `question` added by @charliermarsh on 2024-03-14 15:43_

---

_Comment by @charliermarsh on 2024-03-14 20:02_

The other thing you could do is make it an abstract class so that there's a formal interface. Then, if you decorate the method with `@override`, Ruff will correctly detect that the args are required.

---

_Comment by @charliermarsh on 2024-03-14 20:02_

Hope that's helpful!

---

_Closed by @charliermarsh on 2024-03-14 20:02_

---

_Comment by @matthiasverstraete on 2024-03-15 12:59_

Unfortunately I have no influence over the informal interface. I'll use the `# noqa`.

Thanks for the response.

---

_Comment by @pianofab on 2025-09-11 16:43_

Pylint handles this automatically by recognizing certain prefixes in the function name (_cb and such), avoiding littering one’s code with noqa statements. Callbacks are extremely common for example in WxPython code. It would be great to handle it just like Pylint.

---
