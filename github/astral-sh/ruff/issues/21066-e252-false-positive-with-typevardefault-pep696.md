---
number: 21066
title: "E252: false positive with TypeVarDefault [PEP696] [preview]"
type: issue
state: open
author: randolf-scholz
labels:
  - question
assignees: []
created_at: 2025-10-24T16:52:55Z
updated_at: 2025-10-27T09:53:58Z
url: https://github.com/astral-sh/ruff/issues/21066
synced_at: 2026-01-07T13:12:16-06:00
---

# E252: false positive with TypeVarDefault [PEP696] [preview]

---

_Issue opened by @randolf-scholz on 2025-10-24 16:52_

### Summary

https://play.ruff.rs/7a92d282-f540-41cb-83f1-01fc0f33422e

```python
def foo[T=int]() -> None: ...
# E252 [*] Missing whitespace around parameter equals
```

However, one would expect for [E252](https://docs.astral.sh/ruff/rules/missing-whitespace-around-parameter-equals/) that the same rules that apply to regular function arguments also apply to TypeVars, that is:

```python
def foo[T=int]() -> None: ... # without upper bound
def bar[T: object = int]() -> None: ... # with upper bound
```


### Version

_No response_

---

_Comment by @ntBre on 2025-10-24 19:19_

I personally kind of prefer the space, and it's consistent with our formatter (as noted in your other issue, https://github.com/astral-sh/ruff/issues/18746). I also think we agree with pycodestyle here:

```shell
> pycodestyle - <<EOF
def foo[T=int]() -> None: ... # without upper bound
def bar[T: object = int]() -> None: ... # with upper bound
EOF
stdin:1:10: E225 missing whitespace around operator
stdin:1:10: E252 missing whitespace around parameter equals
stdin:1:11: E252 missing whitespace around parameter equals
stdin:1:30: E261 at least two spaces before inline comment
stdin:2:40: E261 at least two spaces before inline comment
```

---

_Label `question` added by @ntBre on 2025-10-24 19:19_

---

_Comment by @randolf-scholz on 2025-10-25 08:39_

It seems that behavior was introduced in pycodestyle by https://github.com/PyCQA/pycodestyle/commit/8621e318655267c2a6cfa15bfd3f7cc02a60881f without any discussion. As I explained in the other PR, I find this confusing, because it differs from the spacing rules of regular function arguments, and it eats up valuable space without - in my opinion - improving readability.

Especially when you have multiple overloads, readability degradates a lot when they no longer fit on a single line, as I explained in the other issue.

---

_Comment by @randolf-scholz on 2025-10-25 08:42_

And if I disable E252, it no longer reports errors for regular function parameters!

---

_Comment by @MichaReiser on 2025-10-27 08:36_

I don't think I follow the consistency argument between type vars and function arguments. 

```py
def foo[T=int]() -> None: ... # without upper bound
def bar[T: object = int]() -> None: ... # with upper bound


def foo(a: T = 4): ...
```

This seems consistent to me: Ruff enforces a space before and after the `=` following a type (annotation).

Which follows PEP8

> When combining an argument annotation with a default value, however, do use spaces around the = sign:
> ```py
> # Correct:
> def munge(sep: AnyStr = None): ...
> def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
> ```




---

_Comment by @randolf-scholz on 2025-10-27 09:45_

@MichaReiser For regular function parameters spaces are omitted for default values, unless there is a type annotation, e.g. 

```python
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
```

rather than 

```python
def munge(input: AnyStr, sep: AnyStr = None, limit = 1000): ...
```

I'm saying that one would expect the same for type-parameters, so for a type-var with default, but without upper bound:

```python
def foo[X=int](): ...
```

rather than 

```python
def foo[X = int](): ...
```

Both E252 and the ruff formatter want the version with spaces here. The maintainers of `pycodestyle` base this decision on how the code was presented in PEP696, but that PEP doesn't make any explicit style recommendations. 

Personally, I strongly prefer `=` without spaces in this context, for the same reason we use `=` without spaces for un-annotated function parameters. It is visually more clear and saves horizontal space. So I'd love to be able to globally disable E252 for type-parameters (but not the other uses of E252).

Does ruff support sub-error codes? (like E252a, E252b, etc?)



---

_Comment by @MichaReiser on 2025-10-27 09:53_

I see. I don't think there's much to argue other than preferences differ :)

> Does ruff support sub-error codes? (like E252a, E252b, etc?)

No, I also don't think it should. Rule selection is already a very hard problem

I think there's an argument that it should be a separate rule:

> Checks for missing whitespace around the equals sign in an annotated function keyword parameter.

Or we should update the rule documentation. 




---
