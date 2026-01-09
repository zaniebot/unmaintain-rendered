---
number: 11105
title: "[Panic] Accidental Space after function name (instead of opening parenthesis) crashes Ruff"
type: issue
state: closed
author: greywidget
labels:
  - parser
assignees: []
created_at: 2024-04-23T15:06:27Z
updated_at: 2024-04-23T15:31:09Z
url: https://github.com/astral-sh/ruff/issues/11105
synced_at: 2026-01-07T13:12:15-06:00
---

# [Panic] Accidental Space after function name (instead of opening parenthesis) crashes Ruff

---

_Issue opened by @greywidget on 2024-04-23 15:06_

Keywords checked: "function name", "trailing space"

I was starting to define a Django View and accidentally hit "space" instead of "left parenthesis" after View function name:

```
from django.shortcuts import render
from .models import Post

def post<accidental space here>
```

Using Ruff plugin for VSCode. Ruff version: `0.4.0`

### Error
```
Ruff: Lint failed (...

thread 'main' panicked at D:\a\ruff\ruff\crates\ruff_text_size\src\range.rs:48:9:
assertion failed: start.raw <= end.raw
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
)
```

### pyproject.toml relevant settings:
```
[tool.ruff]
line-length = 88

[tool.ruff.lint]
select = ["E", "F", "I"]
ignore = ["E501"]
```

---

_Comment by @AlexWaygood on 2024-04-23 15:21_

Hi, thanks for the report! I think this might already be fixed on the latest version (ruff 0.4.1). Cc. @dhruvmanila 

---

_Label `parser` added by @AlexWaygood on 2024-04-23 15:21_

---

_Comment by @AlexWaygood on 2024-04-23 15:26_

See https://github.com/astral-sh/ruff/issues/11037 and https://github.com/astral-sh/ruff/pull/11032

---

_Comment by @greywidget on 2024-04-23 15:27_

Yes, I see you are correct - it no longer occurs once I upgraded to 0.4.1

Sorry - I'll make sure I upgrade to the latest version next time before raising any issues.

---

_Closed by @greywidget on 2024-04-23 15:28_

---

_Comment by @AlexWaygood on 2024-04-23 15:29_

> Yes, I see you are correct - it no longer occurs once I upgraded to 0.4.1
> 
> Sorry - I'll make sure I upgrade to the latest version next time before raising any issues.

Thanks for confirming! No worries :-)

> Should I close this or is it something you do?

Either is fine ;-)

---

_Comment by @greywidget on 2024-04-23 15:29_

Already fixed in 0.4.1

---

_Comment by @charliermarsh on 2024-04-23 15:31_

Thanks @greywidget

---
