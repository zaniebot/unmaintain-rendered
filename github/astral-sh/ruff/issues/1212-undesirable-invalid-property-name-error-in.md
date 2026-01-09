---
number: 1212
title: Undesirable invalid property name error in convert_typed_dict_functional_to_class
type: issue
state: closed
author: martinlehoux
labels:
  - rule
assignees: []
created_at: 2022-12-12T15:58:37Z
updated_at: 2023-02-22T23:05:26Z
url: https://github.com/astral-sh/ruff/issues/1212
synced_at: 2026-01-07T13:12:14-06:00
---

# Undesirable invalid property name error in convert_typed_dict_functional_to_class

---

_Issue opened by @martinlehoux on 2022-12-12 15:58_

Just adding a reminder, I'll try to get to it soon

When running pyupgrade on my codebase, I got the following error:

```
[2022-12-12][16:54:45][ruff::pyupgrade::plugins::convert_typed_dict_functional_to_class][ERROR] Failed to parse TypedDict: Invalid property name: xsi:noNamespaceSchemaLocation
```

I remember we logged an error because we couldn't upgrade it, but I shouldn't see an error as a user: I know that I can convert this TypedDict to class syntax, because I have weird keys

---

_Comment by @martinlehoux on 2022-12-12 15:59_

Or the user based solution is to ignore this specific TypedDict?

---

_Comment by @charliermarsh on 2022-12-20 01:10_

I think we could flag the error, but not try to upgrade it?

---

_Comment by @martinlehoux on 2022-12-23 21:04_

It all depends on what we expect the user to do: do they have to explicitly ignore the error ? I guess that would be the simplest 

---

_Label `rule` added by @charliermarsh on 2022-12-31 18:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-07 18:16_

---

_Referenced in [astral-sh/ruff#1723](../../astral-sh/ruff/pulls/1723.md) on 2023-01-07 22:48_

---

_Closed by @charliermarsh on 2023-01-07 22:51_

---

_Comment by @sjdemartini on 2023-02-22 22:29_

Assuming I understand the above, it seems the current Ruff behavior here is still not quite desirable. Per the python official docs [here](https://docs.python.org/3/library/typing.html#typing.TypedDict):

> The functional syntax should also be used when any of the keys are not valid [identifiers](https://docs.python.org/3/reference/lexical_analysis.html#identifiers), for example because they are keywords or contain hyphens. Example:
>    ```
>    # raises SyntaxError
>    class Point2D(TypedDict):
>        in: int  # 'in' is a keyword
>        x-y: int  # name with hyphens
>    
>    # OK, functional syntax
>    Point2D = TypedDict('Point2D', {'in': int, 'x-y': int})
>    ```

As such, Ruff's current behavior of showing a violation of "UP013 Convert `Point2D` from `TypedDict` functional to class syntax" in the above functional syntax example seems undesirable, since it's actually the recommended/only way to use `TypedDict`. Rather, Ruff should just silently ignore. (pyupgrade correctly ignores this scenario.) Should I file a new issue for this, or should this one be reopened potentially?

Thank you for this awesome library!

---

_Comment by @charliermarsh on 2023-02-22 22:34_

Ah yeah, I agree with you. Can you create a new issue? I'll fix it real quick.

---

_Referenced in [astral-sh/ruff#3147](../../astral-sh/ruff/issues/3147.md) on 2023-02-22 22:45_

---

_Comment by @sjdemartini on 2023-02-22 22:46_

Yes absolutely, thanks for the quick help! Here's an issue https://github.com/charliermarsh/ruff/issues/3147. Your speed on GitHub rivals the speed of Ruff itself haha

---

_Comment by @charliermarsh on 2023-02-22 23:05_

😅 

---
