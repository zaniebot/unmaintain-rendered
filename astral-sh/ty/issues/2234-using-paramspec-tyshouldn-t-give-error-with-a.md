```yaml
number: 2234
title: "Using `ParamSpec`, tyshouldn't give error with a tuple but should give error with a list as a default value, following the docstring."
type: issue
state: closed
author: hyperkai
labels:
  - question
assignees: []
created_at: 2025-12-27T04:11:42Z
updated_at: 2025-12-27T07:18:34Z
url: https://github.com/astral-sh/ty/issues/2234
synced_at: 2026-01-10T01:56:41Z
```

# Using `ParamSpec`, tyshouldn't give error with a tuple but should give error with a list as a default value, following the docstring.

---

_Issue opened by @hyperkai on 2025-12-27 04:11_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14
- Windows 11

The docstring of [ParamSpec](https://docs.python.org/3/library/typing.html#typing.ParamSpec) shows two examples using the default value with a tuple as shown below:

```python
from typing import ParamSpec

print(help(ParamSpec))
```

> type IntFuncDefault[**P = (int, str)] = Callable[P, int]

> P = ParamSpec('P')
>      DefaultP = ParamSpec('DefaultP', default=(int, str))

But ty gives the error because a tuple is set instead of a list as shown below:

```python
from typing import ParamSpec

                        # ↓ tuple  ↓
type IntFuncDefault[**P = (int, str)] = Callable[P, int] # Error

P = ParamSpec('P')                     # ↓ tuple  ↓
DefaultP = ParamSpec('DefaultP', default=(int, str)) # Error
```

> error[invalid-paramspec]: The default value to `ParamSpec` must be either a list of types, `ParamSpec`, or `...`

Actually, setting a list gets no error as shown below:

```python
from typing import ParamSpec

                        # ↓  list  ↓
type IntFuncDefault[**P = [int, str]] = Callable[P, int] # No error

P = ParamSpec('P')                     # ↓  list  ↓
DefaultP = ParamSpec('DefaultP', default=[int, str]) # No error
```

So using `ParamSpec`, ty shouldn't give error with a tuple but should give error with a list as a default value, following the docstring.

### Version

_No response_

---

_Comment by @dhruvmanila on 2025-12-27 07:11_

I think the docstring is incorrect as the typing spec mentions that it should be a _list_ of types (and not _tuple_ of types):

> ParamSpec defaults are defined using the same syntax as TypeVar s but use a list of types or an ellipsis literal “...” or another in-scope ParamSpec (see [Scoping Rules](https://typing.python.org/en/latest/spec/generics.html#scoping-rules)).
>
> https://typing.python.org/en/latest/spec/generics.html#paramspec-defaults

Other type checkers error on tuple of types as well - [Pyright](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQAKRIREAygtQMYBQP8nKAEkUMAGIBXFFwAi1YEQkkYAbQBUahlAC8UABSoY9AM4wQASgC6OwqXJVqKhvUPWAxFACiIcCD5bdJhZ2Ti49AHIGcPMeOQUlGADGZlYObgi4xWUo%2BgATeSyYbQNREzNzcygPb18gA), [mypy](https://mypy-play.net/?mypy=latest&python=3.14&gist=c1da95c4525a174a58c4e6a2371badf3).

I think we just need to update the docs here: https://github.com/python/cpython/blob/54362898f32bf195db898bfead15784d6ab5831b/Objects/typevarobject.c#L1441-L1492 in the CPython repository.

---

_Label `question` added by @dhruvmanila on 2025-12-27 07:13_

---

_Comment by @dhruvmanila on 2025-12-27 07:18_

I've opened https://github.com/python/cpython/issues/143206, it looks like a good first issue :)

---

_Closed by @dhruvmanila on 2025-12-27 07:18_

---
