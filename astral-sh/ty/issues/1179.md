```yaml
number: 1179
title: "`lru_cache` decorator not supported"
type: issue
state: closed
author: mattangus
labels: []
assignees: []
created_at: 2025-09-13T09:29:43Z
updated_at: 2025-09-15T07:42:43Z
url: https://github.com/astral-sh/ty/issues/1179
synced_at: 2026-01-10T02:06:25Z
```

# `lru_cache` decorator not supported

---

_Issue opened by @mattangus on 2025-09-13 09:29_

### Summary

Adding the `lru_cache` decorator results in an `Unknown` return type. E.g. This code:

```python
from functools import lru_cache

@lru_cache(10)
def fibonacci(n: int) -> int:
     if n in {0, 1}:  # Base case
         return n
     return fibonacci(n - 1) + fibonacci(n - 2)  # Recursive case
     
def main():
    f1 = fibonacci(1)
    reveal_type(f1)
    reveal_type(fibonacci)
```

Reveals type `Unknown` in [ty](https://play.ty.dev/47b425e0-15d2-4148-a3b5-4b9c98fa2936
), and `int` in [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBMCuB2BjALmMAbAzlAlhADmCMlOiLAPqICGiAFgKYCwAUKwAJmU30MAUARgAMASlYATBsBg4ARmHi1EOPvABcueMhFQAtAD5NyNayhmzOafE1QA3kIA0UAQF8NUAMRQAQtUwMoGn9Tc1CoEAZkWBBreBDzCKiYmXlFRGVVPWcdAGoUhSUVa10oACYdTygAJQZEaMwcADcAoOYWMIkpKAhqHHg%2BERN282ABKABefLSMgTFhswjm6nQKZABPfH5RudDFhmXVja25AvScOaA).

### Version

ty 0.0.1-alpha.20

---

_Comment by @sharkdp on 2025-09-15 07:41_

Thank you for reporting this. Our generics solver is currently not yet capable of solving for `_T` when calling
```py
Callable[[Callable[..., _T]], _lru_cache_wrapper[_T]]
```
(the return type of `lru_cache`) with `fibonacci` of type `int -> int`, and so it returns `_lru_cache_wrapper[Unknown]` instead of `_lru_cache_wrapper[int]`. See #623.

---

_Closed by @sharkdp on 2025-09-15 07:41_

---
