```yaml
number: 722
title: Enhancement request - fixers for pycodestyle rules
type: issue
state: closed
author: peterjc
labels: []
assignees: []
created_at: 2022-11-13T18:04:37Z
updated_at: 2022-11-20T03:42:45Z
url: https://github.com/astral-sh/ruff/issues/722
synced_at: 2026-01-12T15:54:40Z
```

# Enhancement request - fixers for pycodestyle rules

---

_@peterjc_

Many of the pycodestyle rules ruff can detect ought to be easy to fix:

https://github.com/charliermarsh/ruff#pycodestyle

e.g. E713 could replace ``not x in y`` with ``x not in y`` and similar.

Tested with ruff 0.0.114



---

_Comment by @harupy on 2022-11-14 01:15_

+1. I can work on this.

---

_Label `enhancement` added by @charliermarsh on 2022-11-14 18:14_

---

_Comment by @harupy on 2022-11-19 11:59_

Are there any other auto-fixable checks? Is E731 the final one?

---

_Comment by @harupy on 2022-11-19 12:03_

Is `DoNotUseBareExcept` auto-fixable?

Fix:

```python
try
    ...
except:
    ...
```

to:

```python
try
    ...
except Exception:
    ...
```

---

_Comment by @JonathanPlasse on 2022-11-19 12:39_

> Is `DoNotUseBareExcept` auto-fixable?
> 
> Fix:
> 
> ```python
> try
>     ...
> except:
>     ...
> ```
> 
> to:
> 
> ```python
> try
>     ...
> except Exception:
>     ...
> ```

No, this is still bad. `flake8-blind-except` consider this an error.

---

_Comment by @charliermarsh on 2022-11-20 00:51_

@harupy - Probably ok to close? Do you agree?

---

_Comment by @harupy on 2022-11-20 03:33_

Agreed, let's close this.

---

_Closed by @charliermarsh on 2022-11-20 03:42_

---
