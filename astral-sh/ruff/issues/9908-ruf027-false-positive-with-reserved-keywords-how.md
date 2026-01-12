```yaml
number: 9908
title: RUF027 false positive with reserved keywords, how to handle?
type: issue
state: closed
author: sshishov
labels: []
assignees: []
created_at: 2024-02-09T10:51:05Z
updated_at: 2024-02-10T23:53:44Z
url: https://github.com/astral-sh/ruff/issues/9908
synced_at: 2026-01-12T15:54:49Z
```

# RUF027 false positive with reserved keywords, how to handle?

---

_@sshishov_

Hi,

I would like to ask how to handle the cases where the "variable" for interpolation is used in the constant or other variable beforehand, example below

```python
CONSTANT_CACHE_KEY = 'my:{id}:here'  # <-- RUF027 is triggered here (Possible f-string without an `f` prefix)

def some_function(value: str) -> None:
    cache_key = CONSTANT_CACHE_KEY.format(id=value)
    ...  # later on we use this cache key and `value` here is like a variable for cache key
```

I have tried to "hack" slightly by using:
```python
CONSTANT_CACHE_KEY = f'my:{{id}}:here'  # <-- F541 is triggered here (f-string without any placeholders)
```

**UPDATE:** this is happening only with reserved keywords like `id` or `filter`. If variable does not exist, then this error is not raised.

Should it be somehow treated or handled? Or for this cases should we add `# noqa: RUF027`? Or maybe the good suggestion would be to rename the variable inside the string from `id` to `object_id`?

ruff version: 0.2.1

---

_Renamed from "RUF027 false positive with constants, how to handle?" to "RUF027 false positive with reserved keywords, how to handle?" by @sshishov on 2024-02-09 10:59_

---

_Comment by @sshishov on 2024-02-09 11:09_

Just a side note, we have updated all usages of `prefix:{id}:suffix` constants to `prefix:{object_id}:suffix` and now we do not have any issues with amazing **ruff** 🎉 

---

_Comment by @Avasam on 2024-02-09 15:49_

As a precision, `id`, `filter`, etc., are not reserved keywords, they are builtins (global names from the stdlib).

RUF027's heuristic detects when you try to use an existing symbols in brackets.

```python
my_value = 5
message = "I have {my_value} things"
print(message)
```

But since `id` is always defined, it'll always trigger in your case.
It's just like https://docs.astral.sh/ruff/rules/builtin-variable-shadowing/

Maybe RUF027 could respect https://docs.astral.sh/ruff/settings/#lint_flake8-builtins_builtins-ignorelist ?



---

_Comment by @AlexWaygood on 2024-02-09 16:00_

I think this is already fixed on `main`, due to:

- #9849

It should be included in the  next release!

---

_Comment by @charliermarsh on 2024-02-10 23:53_

👍 I believe this will be fixed in the next release -- sorry about that!

---

_Closed by @charliermarsh on 2024-02-10 23:53_

---
