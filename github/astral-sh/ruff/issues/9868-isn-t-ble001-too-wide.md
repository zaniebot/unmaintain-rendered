---
number: 9868
title: "Isn't BLE001 too wide?"
type: issue
state: open
author: adrien-berchet
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-02-06T22:45:41Z
updated_at: 2024-11-27T11:09:20Z
url: https://github.com/astral-sh/ruff/issues/9868
synced_at: 2026-01-07T13:12:15-06:00
---

# Isn't BLE001 too wide?

---

_Issue opened by @adrien-berchet on 2024-02-06 22:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
As far as I understand, the rule `BLE001` is triggered by both `try: ... except BaseException: ...` and `try: ... except Exception: ...` which is not consistent with the related PEP8 section (https://peps.python.org/pep-0008/#programming-recommendations) which states that we should not try to catch a `BaseException` (mainly because it would also catch `SystemExit` and `KeyboardInterrupt` exceptions), but nothing is mentioned about `Exception`. Also, the behavior is not consistent with its description which states this:
![image](https://github.com/astral-sh/ruff/assets/11536226/d492fbfb-90ee-4788-bd97-74a99d4970aa)
And the exception hierarchy (https://docs.python.org/3/library/exceptions.html#exception-hierarchy) clearly shows that `KeyboardInterrupt` and `SystemExit` derive from the `BaseException` class, not from `Exception`. So the following code should not trigger this error:
```python
try:
    print("hello")
except Exception as error:
    print(error)
```
But running `ruff check hello.py --select BLE001` triggers `BLE001`.

Am I missing something?
Anyway, it's quite often that I need to catch a 'generic' error (to do something when 'anything bad' happens) and that I end up ignoring this error, so I think it's not relevant to mark it as an error.

---

_Comment by @zanieb on 2024-02-07 14:41_

I agree with the sentiment here. There are kind of two ideas at work here:

1. You should have use a bare `except:` or `except BaseException:` without re-raising because it is dangerous for proper program behavior
2. You should not use `except Exception` because it is best practice to declare which exception cases you care about

I think these should be separate rules as I would _always_ have 1. on but I think 2. is more of a matter of preference.

---

_Label `rule` added by @zanieb on 2024-02-07 14:41_

---

_Label `needs-decision` added by @zanieb on 2024-02-07 14:41_

---

_Comment by @adrien-berchet on 2024-02-07 15:46_

I completely agree with that, thanks for your feedback!

---

_Comment by @AlexWaygood on 2024-02-07 16:35_

It's pretty impossible to come up with a rule that *never* has false positives for this kind of thing. I've written and reviewed code where the only correct thing to do is to do `except BaseException: pass` e.g. in https://github.com/python/mypy/blob/780a29d0b1b93848d7ce3faf8e819a6cc140b2e5/mypy/stubtest.py#L1716-L1726, the code is dynamically importing unknown modules at runtime, so needs to catch `BaseException` as there's a chance some module might raise `SystemExit` when you try to import it. (That's not just theoretical -- we changed that line of code after that actually happened.)

I think it makes sense to lint against both `except BaseException` and `except Exception`, but have them be different rules. Both can be common mistakes made by Python beginners, so I think it makes sense to disallow both of them. But the former is even more likely to be a mistake than the latter, so it makes sense for them to be separate rules.

I would also probably not flag at all places where the error is clearly being handled in some way, like the OP's example. In situations like OP's and the ones below, where the error is being handled and logged in some way, it's arguably clear that the user is intentionally catching all errors. I see the current rule already makes a specific exception for `except BaseException` if the exception is passed to `logging.error()`. But I'd be inclined to make that more general; I'm not sure ruff should take issue with any of these:

```py
try:
    do_something()
except BaseException as e:
    print(e)

try:
    something_else()
except BaseException as e:
    logging.error(e)

try:
    third_thing()
except BaseException as e:
    handle_the_error(e)
```

---

_Comment by @FichteFoll on 2024-09-17 16:37_

Flask (and Quart) export an application-wide logger under `current_app.logger`, which ruff does not recognize as an allowed blind except case when used with `.exception` in the `except` block. Perhaps this could be added to the list?

---

_Comment by @AlexWaygood on 2024-09-17 17:53_

@FichteFoll does this setting maybe help address that problem? https://docs.astral.sh/ruff/settings/#lint_logger-objects

---

_Comment by @FichteFoll on 2024-09-18 09:31_

Thanks for the pointer, that did indeed help. Perhaps it would be a good idea to mention that [on the BLE001 documentation page](https://docs.astral.sh/ruff/rules/blind-except/).

---

_Comment by @sasanjac on 2024-11-27 11:09_

This has to be two different rules because semantically, these are two different things:
One is used as a hidden control flow, the other is the base class for "real" exceptions.
Consider the following example: I have an app with a running main loop which should not close unexpectedly, so I want to handle BaseExceptions sans Exception. Not doing so would result in my app not responding to external signals.
However, subclasses of Exception are exceptions that occur within my app and may not know all of them beforehand, so I might want to log them so my loop does not close and implement handling them later. 

---
