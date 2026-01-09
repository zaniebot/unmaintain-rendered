---
number: 5421
title: E731 auto-fix is too aggressive
type: issue
state: closed
author: adampauls
labels:
  - question
assignees: []
created_at: 2023-06-28T15:59:32Z
updated_at: 2023-06-29T06:20:31Z
url: https://github.com/astral-sh/ruff/issues/5421
synced_at: 2026-01-07T13:12:15-06:00
---

# E731 auto-fix is too aggressive

---

_Issue opened by @adampauls on 2023-06-28 15:59_

Consider the following code:

```python
@dataclass
class Foo:
    my_lambda: Callable[[], int]


def func(arg: Foo, flag: bool):
    my_lambda: Callable[[], int]
    if flag:
        my_lambda = arg.my_lambda
    else:
        my_lambda = lambda: 5  # noqa: E731

    return my_lambda()
```

Ruff will autofix the `noqa` line, but that introduces a bug, at least I think it does. Certainly `pyright` complains.

EDIT:
After the autofix, the code is

```python
@dataclass
class Foo:
    my_lambda: Callable[[], int]


def func(arg: Foo, flag: bool) -> int:
    my_lambda: Callable[[], int]
    if flag:
        my_lambda = arg.my_lambda
    else:
        def my_lambda():
            return 5

    return my_lambda()
``` 


---

_Comment by @zanieb on 2023-06-28 16:04_

@adampauls can you please include the code after the autofix and the pyright complaint?

---

_Label `question` added by @charliermarsh on 2023-06-28 18:50_

---

_Comment by @adampauls on 2023-06-28 20:43_

Done

---

_Comment by @zanieb on 2023-06-28 20:52_

@adampauls that fix looks fine to me and runs correctly

```python
> func(Foo(my_lambda=lambda: 1), True)
1

> func(Foo(my_lambda=lambda: 1), False)
5
```

What's the bug / pyright complaint?

---

_Comment by @adampauls on 2023-06-28 21:17_

Pyright says

```
error: Declaration "my_lambda" is obscured by a declaration of the same name
```

I guess you can argue that `pyright` is issuing an incorrect error here, but to my eyes the code becomes worse after the fix here. I had imagined that the ruff rule/fix was spiritually targeted towards the case where assigning a lambda is just uglier syntax for a `def`, whereas here, "assigning a lambda" really is a meaningful assignment to an existing symbol instead of  declaration of a new one. If this behavior is intended, feel free to close and I'll just ignore the rule. 

---

_Comment by @charliermarsh on 2023-06-28 21:23_

This might be a case where there could be another suggestion for refactoring the code. Is it possible to share a more realistic snippet?

---

_Comment by @adampauls on 2023-06-28 21:25_

Here's a link to the [original code](https://github.com/microsoft/semantic_parsing_with_constrained_lm/blob/65fcf83d83c0c60fd63107d0274f4c899489c3b0/src/semantic_parsing_with_constrained_lm/configs/benchclamp_gpt3_config.py#L94-L99)

---

_Comment by @charliermarsh on 2023-06-28 21:35_

Thanks :) Hmm, perhaps we could avoid flagging E731 when the variable already exists as an annotation? E.g., based on the `partial_parse_builder: Callable[[Datum], PartialParse]`.

---

_Comment by @adampauls on 2023-06-28 22:07_

FWIW I don't even mind the lint error, I like opinionated linters. I just don't like opinionated autofixers, since you might not even realize that code was changed behind the scenes if you're running it over lots of code. If it were me, I would disable the autofix if the variable is assigned or declared anywhere other than the place where it is assigned to a lambda (before or after).

---

_Comment by @charliermarsh on 2023-06-29 00:27_

I think that's reasonable. We could mark it as a manual fix in that case.

---

_Referenced in [astral-sh/ruff#5430](../../astral-sh/ruff/pulls/5430.md) on 2023-06-29 01:40_

---

_Closed by @charliermarsh on 2023-06-29 02:00_

---

_Comment by @adampauls on 2023-06-29 06:20_

Thanks! 

---
