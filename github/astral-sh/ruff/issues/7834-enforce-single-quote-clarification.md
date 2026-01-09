---
number: 7834
title: Enforce single quote - clarification
type: issue
state: closed
author: Auric-Manteo
labels:
  - question
assignees: []
created_at: 2023-10-06T08:24:34Z
updated_at: 2024-05-14T06:56:40Z
url: https://github.com/astral-sh/ruff/issues/7834
synced_at: 2026-01-07T13:12:15-06:00
---

# Enforce single quote - clarification

---

_Issue opened by @Auric-Manteo on 2023-10-06 08:24_

I am trying to get ruff to complain about using double quotes.
In this example I want to have single quotes instead of double quotes: `my_var="text"`
Currently when I run ruff check it does not complain if I use double quotes or single quotes.
If this does not work I don't quite get hat the inline-quotes (I never heard the term  inline quote. Is that "'" an inline quote? :D) and quote-style is for.
Could you clarify?

>  ruff 0.0.292

pyproject.toml 

      [tool.ruff]
      line-length = 120
      
      [tool.ruff.flake8-quotes]
      docstring-quotes = "double"
      inline-quotes = "single"
      
      [tool.ruff.format]
      # Prefer single quotes over double quotes
      quote-style = "single"
      
      [tool.black]
      line-length = 120
      skip-string-normalization = true
    


---

_Comment by @dhruvmanila on 2023-10-06 13:25_

Hey, is this related to the linter? If so, you'll have to select the rules explicitly:

```toml
[tool.ruff]
extend-select = ["Q"]
```

The `inline-quotes` settings is for the inline strings which are basically every string apart from the docstrings.

---

_Label `question` added by @zanieb on 2023-10-06 13:54_

---

_Comment by @Auric-Manteo on 2023-10-09 09:37_

Fantastic, thank you @dhruvmanila!

---

_Closed by @Auric-Manteo on 2023-10-09 09:37_

---

_Referenced in [OpenHands/OpenHands#1204](../../OpenHands/OpenHands/pulls/1204.md) on 2024-04-25 00:21_

---

_Comment by @Borda on 2024-04-29 23:02_

@dhruvmanila we may have a similar issue; I have set all as suggested:
```toml
[lint]
select = [
    ...
    "Q",
]

flake8-quotes = {inline-quotes = "single"}
```
But still, the single quotes are not enforced, for example [here](https://github.com/OpenDevin/OpenDevin/pull/1425#discussion_r1582056596)
this line seems to be untouched:
```py
logger.info(
            f"Connecting to {username}@{hostname} via ssh. If you encounter any issues, you can try `ssh -v -p {self._ssh_port} {username}@{hostname}` with the password '{self._ssh_password}' and report the issue on GitHub."
        )
```

---

_Referenced in [OpenHands/OpenHands#1425](../../OpenHands/OpenHands/pulls/1425.md) on 2024-04-29 23:30_

---

_Referenced in [astral-sh/ruff#11209](../../astral-sh/ruff/issues/11209.md) on 2024-04-29 23:39_

---
