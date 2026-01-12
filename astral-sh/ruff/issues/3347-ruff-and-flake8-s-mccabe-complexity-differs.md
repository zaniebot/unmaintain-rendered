```yaml
number: 3347
title: "Ruff and Flake8's McCabe complexity differs"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-04T21:06:07Z
updated_at: 2023-03-14T18:40:35Z
url: https://github.com/astral-sh/ruff/issues/3347
synced_at: 2026-01-12T15:54:43Z
```

# Ruff and Flake8's McCabe complexity differs

---

_@charliermarsh_

From Discord: https://gitlab.com/ppentchev/temp-20230302-ruff-complexity

---

_Label `bug` added by @charliermarsh on 2023-03-04 21:06_

---

_Comment by @charliermarsh on 2023-03-04 21:26_

Reproduced.

---

_Comment by @charliermarsh on 2023-03-04 22:07_

Here's a more minimal repro -- Ruff calls this 3, Flake8 calls this 2:

```py
def process_detect_lines():
    try:
        pass
    finally:
        if res:
            errors.append(f"Non-zero exit code {res}")
```

I may have to actually learn how this works, because I thought that would've been 3.


---

_Comment by @charliermarsh on 2023-03-04 22:09_

I guess it makes sense that it's 2 if you say that the `finally` has to trigger, but then why is this 2 also?

```py
def process_detect_lines():
    try:
        pass
    finally:
        pass
```

---

_Comment by @JonathanPlasse on 2023-03-04 22:30_

Maybe this is a bug in flake8.

---

_Comment by @ashwinrajeev on 2023-03-09 04:43_

> I guess it makes sense that it's 2 if you say that the `finally` has to trigger, but then why is this 2 also?
> 
> ```python
> def process_detect_lines():
>     try:
>         pass
>     finally:
>         pass
> ```

I think this is a bug in flake8. This is supposed to be 1, not 2. [Radon](https://pypi.org/project/radon/) give this code 1 cc.

https://radon.readthedocs.io/en/latest/intro.html#cyclomatic-complexity

---

_Comment by @charliermarsh on 2023-03-09 04:47_

Oh cool, I'll take another look at our implementation with those guidelines in mind.

---

_Comment by @akx on 2023-03-10 08:53_

I also noticed this when adding Ruff to HA – didn't think to report it back then though! https://github.com/home-assistant/core/blob/fde205c158b704683650cec1d5832ca0858bbd21/pyproject.toml#L293-L302

---

_Closed by @charliermarsh on 2023-03-14 18:40_

---
