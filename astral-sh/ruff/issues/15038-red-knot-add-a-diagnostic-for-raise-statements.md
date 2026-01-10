```yaml
number: 15038
title: "[red-knot] Add a diagnostic for `raise` statements used with non-exceptions"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-12-17T15:29:15Z
updated_at: 2024-12-18T18:31:26Z
url: https://github.com/astral-sh/ruff/issues/15038
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Add a diagnostic for `raise` statements used with non-exceptions

---

_Issue opened by @AlexWaygood on 2024-12-17 15:29_

Red-knot currently doesn't emit a diagnostic on this snippet:

```py
raise 42
```

But we should, since `42` is not a valid object to be used in a `raise` statement:

```pycon
>>> raise 42
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    raise 42
TypeError: exceptions must derive from BaseException
```

The rule is that the object used in a `raise` statement must have a type that is assignable to the type `BaseException | type[BaseException]`, since both of the following work:

```pycon
>>> raise ValueError
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    raise ValueError
ValueError
>>> raise ValueError()
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    raise ValueError()
ValueError
```

We need to add some extra logic to this branch here: https://github.com/astral-sh/ruff/blob/c9fdb1f5e3a4d0d60b4537741f2c9c19e2426038/crates/red_knot_python_semantic/src/types/infer.rs#L2193-L2201

I think this will probably need to be a new rule added to https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/diagnostic.rs ? I don't think it fits neatly into any other rule. The alternative might be to make the existing `INVALID_EXCEPTION_CAUGHT` rule broader (and rename it).

---

_Label `help wanted` added by @AlexWaygood on 2024-12-17 15:29_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-17 15:29_

---

_Comment by @MichaReiser on 2024-12-17 15:31_

> The alternative might be to make the existing INVALID_EXCEPTION_CAUGHT rule broader (and rename it).

That was also my first reaction. It's kind of the same problem. 

---

_Comment by @AlexWaygood on 2024-12-17 17:37_

> > The alternative might be to make the existing INVALID_EXCEPTION_CAUGHT rule broader (and rename it).
> 
> That was also my first reaction. It's kind of the same problem.

The same genre of problem, for sure. Though there's a subtle difference between the rules for raising exceptions and the rules for catching exceptions:
- If you're raising an exception, the type must be assignable to `BaseException | type[BaseException]`
- but if you're catching an exception the type must be assignable to `type[BaseException] | tuple[type[BaseException], ...]`

If we bundle them together into one rule, it might be harder to explain these details in the rule's documentation.

---

_Comment by @AlexWaygood on 2024-12-17 17:38_

I don't have a strong opinion, though. Any suggestions for what we might call the rule if we combined the two into one one rule?

---

_Comment by @InSyncWithFoo on 2024-12-17 20:05_

Survey:

* [Pyright](https://pyright-play.net/?pyrightVersion=1.1.390&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=C4JwngXAsAUABAuAHAhgZzbApgDwMZZLBwAMECAxHCIQPYjADiWAdliCgDYAqYSWASQwBXLJniJUGWLA4BLNFlKVqdBszYcefQSLGwgA): `reportGeneralTypeIssues`
* [Mypy](https://mypy-play.net/?mypy=latest&python=3.13&flags=strict&gist=ea92013ec1bbcac8ee093f4ca3ee02da): `misc`
* [Pyre](https://pyre-check.org/play?input=try%3A%0A%20%20%20%20pass%0Aexcept%200%3A%20%20%23%20misc%0A%20%20%20%20pass%0A%0Araise%200%20%20%23%20misc%0A): `Invalid except clause [66]` and `Invalid Exception [48]`
* [Pytype](https://github.com/user-attachments/assets/06940fd3-558c-4dcf-afe8-0eb644c8d12c): No errors
* Ruff: [`raise-literal`](https://docs.astral.sh/ruff/rules/raise-literal/) and [`except-with-non-exception-classes`](https://docs.astral.sh/ruff/rules/except-with-non-exception-classes/)

I think `INVALID_EXCEPTION` is good enough. Each diagnostic can then customize the message.

---

_Closed by @AlexWaygood on 2024-12-18 18:31_

---
