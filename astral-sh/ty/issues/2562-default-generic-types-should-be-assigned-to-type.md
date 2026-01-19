```yaml
number: 2562
title: Default generic types should be assigned to type hints?
type: issue
state: closed
author: gabriel-abn
labels:
  - question
assignees: []
created_at: 2026-01-19T12:59:08Z
updated_at: 2026-01-19T13:11:45Z
url: https://github.com/astral-sh/ty/issues/2562
synced_at: 2026-01-19T13:34:13Z
```

# Default generic types should be assigned to type hints?

---

_@gabriel-abn_

### Question

Hi guys... me again :v

the problem/question is really straightforward: 
shouldn't generics with a default type hint to this specific type when no overlap specified?

a really simple example:

```python
from abc import ABC

class Product[T: float](ABC):
	amount: T

	def __init__(self, amount: T) -> None:
		self.amount = amount

	def get_amount(self) -> T:
		return self.amount


class Course(Product, ABC):
	course_name: str

	def __init__(self, amount: float, course_name: str) -> None:
		super().__init__(amount)
		self.course_name = course_name


test = Course(amount=100, course_name="Mathematics")
reveal_type(test.get_amount())  # revealed: Unknown
```

when looking for the type hint on `.get_amount()` method, it shows that the return type is `Unknown`.

<img width="716" height="527" alt="Image" src="https://github.com/user-attachments/assets/22aa36b0-5d6f-4f7b-8201-f5fe1bc0d041" /> 

just checked what it hints over the superclass constructor and it's set to Unknown as well...

---

when i don't define a overlap type does it really set to unknown and ignore the default generic type?

### Version

0.0.11

---

_Label `question` added by @gabriel-abn on 2026-01-19 12:59_

---

_Comment by @AlexWaygood on 2026-01-19 13:11_

Hi! I think ty is correct on this one -- [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=88cd195054dad69056a84b40fe708e27), [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBAhgRgYygSwgBzCALlAggIQGEAoYhAGxgGcqoAFcAEwFcEsBtAFQC4phywMLAF0AFAUIBKbsQCQMCGGYA7LL06lZjAKbAoAfX3JlyLIdFVt5YABpYilWqidJUALQA%2BKADkwy7TKyspbWAHQKSqpQALz2kViaOnoA5tpmEY4WVsCuns6BsiBpzCDKUCHA4Q6qpGSUNFCESiCWogxgLGx2EtJyCM2W%2BsoKAeVYIIm6BkYmZvpZ1nYZqrz8glh2-SWDwxCjVOO5Xr7%2BBVTMaNogopKhhsam5stYknLB2aFbLdpDIzFQXx2I1qWG0B3%2BTW22lEz2iAEYAAwIzYDH67bTRABEAFkhAALbQQITIBBUTGvIoAN20MHI%2BiwAE9LqJQQdQql0tUsDdXkA) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEq2AxnRDcbpQC50CCAQgGEAOulFMoqOHDoAFapgCuTLgG0AKojpgouVFwC6ACgGCAlIlHCuqGrkXouW9VbFdMMMHQD63iOgguXyM4GCgwABoGOwcnOnUzOgBaAD46ADlcdBhLN2tQ8MJbe0c6AF5okq5Xaw8vAHMYIOLYkLCwRNT43OtrSibFSnQ6ArAimMca9AkpGUF7SlCjeVwlFSjTCysuJgXQ73RbHJGuShr3Tx8-AKDvNvColsctHT0uKN3B-cOaY7hTzppTLZHrVLhwRTEGCUIxmQi%2BfyBYJPLhmbb5dqET6LGAHI7lOjY75HKZceA8CrzL4wIwosoARgADIyPntcT8YGVhCAALL6AAWMBo%2BggTDg3LR6H6ADcYKgoN4uKQoUYyf9CI1mhMuLDJSAIiBFFxoHASORECAAMR0ACqxqggVI2gcKggWXFYiwlzAnGFQXQiho2GhRnwWn8qOSaX%2BlFydHjdH6XEGwzA3PSgeDsbowHwAF9uaJ9SAyP0dKRCFxaFAKNbZKQy1AnWgsHh8ISspB6oMRVlCKJrQBlGAwOj8rhcYhwRAAehnpc8TcInHqM5g6BnmFwYpnu3QXZ7xqyM%2B0nAY0tQ0EYsA7%2B4g3coveGuGIR-QptEZC4-KySVlizdYYKm5ABmQh6QAJkLdAQDzA1UFdWUADFoBgCgWxwAgzVgoA) all agree with us here.

The issue with your code as it is currently written is that your `Course` class does not provide type arguments to `Product` when it inherits from `Product`. For example, you could have written `class Course(Product[float], ABC): ...`, and then ty [would have inferred the type correctly](https://play.ty.dev/a9764cac-0eed-4d2d-aff6-6cbfc98fc4a0). Because you haven't provided any type arguments here, ty applies the "default specialization" of `Product` when it considers the type that `Course` should be understood as inheriting from. In this case, the default specialization of `Product` is `Product[Unknown]` (or `Product[Any]`, which means the same thing).

The reason why the default specialization of `Product` is `Product[Unknown]` rather than `Product[float]` is because `Product` is generic over a type variable `T` that has an _upper bound_ (`float`) but does not have a _default_. If you want the default specialization of `Product` to be `Product[float]` rather than `Product[Unknown]`, you would have to write the class like this:

```py
from abc import ABC

class Product[T: float = float](ABC):
	amount: T

	def __init__(self, amount: T) -> None:
		self.amount = amount

	def get_amount(self) -> T:
		return self.amount
```

Mypy and pyright both offer disabled-by-default rules that you can opt into that will warn you if you don't provide type arguments to a generic class, since it's often unexpected and undesirable for the default specialization to be implicitly applied like this. We intend to add such a rule at some point too -- you can follow https://github.com/astral-sh/ty/issues/2442 for updates on that!

---

_Closed by @AlexWaygood on 2026-01-19 13:11_

---
