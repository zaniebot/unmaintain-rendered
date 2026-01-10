```yaml
number: 9780
title: "S308 (mark_safe) doesn't detects decorator usage and imports from another place"
type: issue
state: closed
author: ashrub-holvi
labels:
  - rule
assignees: []
created_at: 2024-02-02T09:43:00Z
updated_at: 2024-02-08T04:10:56Z
url: https://github.com/astral-sh/ruff/issues/9780
synced_at: 2026-01-10T11:09:52Z
```

# S308 (mark_safe) doesn't detects decorator usage and imports from another place

---

_Issue opened by @ashrub-holvi on 2024-02-02 09:43_

Hi, thank you for the cool project!

I am looking to [suspicious-mark-safe-usage (S308)](https://docs.astral.sh/ruff/rules/suspicious-mark-safe-usage/) check. [Here](https://github.com/astral-sh/ruff/issues/1646) I see it's implemented, but something named "S703: django_mark_safe" is not, not sure what does it means, but looks like S308 works only if `mark_safe` is imported from  [django.utils.safestring](https://docs.djangoproject.com/en/5.0/ref/utils/#module-django.utils.safestring) and used as a function:
1. With  `mark_safe` used as a function:
```
from django.utils.safestring import SafeString
from django.utils.safestring import mark_safe

def some_func():
    return mark_safe('<script>alert("evil!")</script>')  # oh no

print(type(some_func()) is SafeString)
```
it works fine:
```
ruff --select S308 test.py 
test.py:7:12: S308 Use of `mark_safe` may expose cross-site scripting vulnerabilities
Found 1 error.
```

2. With  `mark_safe` used as a decorator:
```
from django.utils.safestring import SafeString
from django.utils.safestring import mark_safe

@mark_safe
def some_func():
    return '<script>alert("evil!")</script>'  # oh no

print(type(some_func()) is SafeString)
```
it doesn't raise the error:
```
ruff --select S308 test.py
```

3. With function imported from [django.utils.html](https://docs.djangoproject.com/en/5.0/ref/utils/#module-django.utils.html), might be it's wrong way to import it, but it works and we have a lot of such usages in old code.
```
from django.utils.safestring import SafeString
from django.utils.html import mark_safe

def some_func():
    return mark_safe('<script>alert("evil!")</script>')  # oh no

print(type(some_func()) is SafeString)
```
there is no errors:
```
ruff --select S308 test.py
```

4. With decorator imported from django.utils.html
```
from django.utils.safestring import SafeString
from django.utils.html import mark_safe

@mark_safe
def some_func():
    return '<script>alert("evil!")</script>'  # oh no

print(type(some_func()) is SafeString)
```
also no errors:
```
ruff --select S308 test.py
```

So, perhaps this check can be improved, I tried to looks to [code](https://github.com/astral-sh/ruff/blob/467c091382fa60243e261417b4620aeca4012bf5/crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs#L851), but for the first look I understand nothing ) Rust is only in far away future plan for me.

```
ruff --version
ruff 0.1.15
```


---

_Comment by @charliermarsh on 2024-02-05 00:38_

Interesting, thanks. We should be able to add these diagnostics to the existing rule.

---

_Label `rule` added by @charliermarsh on 2024-02-05 00:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-08 03:48_

---

_Closed by @charliermarsh on 2024-02-08 04:10_

---

_Comment by @charliermarsh on 2024-02-08 04:10_

Fixed in https://github.com/astral-sh/ruff/pull/9887 -- thank you!

---
