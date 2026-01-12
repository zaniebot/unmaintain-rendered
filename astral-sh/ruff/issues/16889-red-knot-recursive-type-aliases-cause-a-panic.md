```yaml
number: 16889
title: "[red-knot] Recursive type aliases cause a panic"
type: issue
state: closed
author: cake-monotone
labels:
  - ty
assignees: []
created_at: 2025-03-21T09:19:20Z
updated_at: 2025-05-07T15:22:52Z
url: https://github.com/astral-sh/ruff/issues/16889
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Recursive type aliases cause a panic

---

_@cake-monotone_

### Summary


It's a relatively uncommon case, but if type aliases refer to each other recursively, it can cause a panic.  
You can find an example in the official Python documentation: https://docs.python.org/3/library/typing.html#typing.TypeAliasType.__value__  
Even though it doesn't raise an error at runtime, it still needs to be handled properly.

```python
type A = B
type B = A

a: A
reveal_type(A)  # panicked!
```

### Version

_No response_

---

_Comment by @cake-monotone on 2025-03-21 09:20_

https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFC2FQCCUAvFAEKOLNcdW9ekVKUafABQsAlPSA

---

_Label `red-knot` added by @MichaReiser on 2025-03-21 09:23_

---

_Comment by @MichaReiser on 2025-03-21 09:24_

Thanks. This is another case where we need to add Salsa cycle handling. You can open the console in the [playground](https://playknot.ruff.rs/fbd7a26c-25b4-4e72-b53c-fb0aabc81f57) to see the backtrace).

---

_Comment by @AlexWaygood on 2025-03-21 09:31_

This is one of the panics listed in https://github.com/astral-sh/ty/issues/228 (listed under Seed 0)

---

_Comment by @cake-monotone on 2025-03-21 09:33_

Ah! Then, should we close this issue for now?
Or would it be better to keep it open to track the fix for this problem?

---

_Comment by @AlexWaygood on 2025-03-21 09:35_

I think we can probably close this one for now, since we've already started working through the panics in the other issue :-) but thank you!

---

_Closed by @AlexWaygood on 2025-03-21 09:35_

---
