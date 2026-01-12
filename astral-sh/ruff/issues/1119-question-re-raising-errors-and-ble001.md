```yaml
number: 1119
title: "[Question] Re-raising errors and BLE001"
type: issue
state: closed
author: notypecheck
labels: []
assignees: []
created_at: 2022-12-07T13:16:51Z
updated_at: 2022-12-07T15:55:47Z
url: https://github.com/astral-sh/ruff/issues/1119
synced_at: 2026-01-12T15:54:41Z
```

# [Question] Re-raising errors and BLE001

---

_@notypecheck_

Not sure if this is the right place to ask since this is a check from flake8-blind-except, but:
Using this check is a bit hard in some cases without ignoring it, for example when we want to report an exception and explicitly re-raise it:
```py
try:
    return handler(request)
except Exception as e:
    sentry_sdk.capture_exception(e)
    raise
```

In my opinion catching `Exception` should be ok as long as you re-raise it 🤔 

---

_Comment by @charliermarsh on 2022-12-07 14:32_

I think I agree with this.

---

_Comment by @charliermarsh on 2022-12-07 14:35_

Some discussion here about this being a good idea: https://github.com/PyCQA/flake8-bugbear/issues/174

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-07 14:40_

---

_Comment by @JonathanPlasse on 2022-12-07 15:26_

I am up for it.
Based on PyCQA/flake8-bugbear#174, we would forbid `BaseException` no matter what and forbid `Exception` if the exception is not re-raised.

---

_Comment by @charliermarsh on 2022-12-07 15:36_

I ended up implementing it here: https://github.com/charliermarsh/ruff/pull/1124. It only detects the simple case of a `raise` in the immediate exception handler body. We could improve it, but seems complex and not sure how common that is.


---

_Closed by @charliermarsh on 2022-12-07 15:37_

---

_Comment by @JonathanPlasse on 2022-12-07 15:53_

It has been implemented in [flake8-trio](https://github.com/Zac-HD/flake8-trio/blob/5072c7d9e4afa340b94ef98fa901afdd563cac65/flake8_trio.py#L606-L608).
We could use the same approach.

---

_Comment by @charliermarsh on 2022-12-07 15:55_

Yeah I just didn't feel it was worth the complexity vs. the common case right now vs. some other things I need to get done. You're very welcome to work on it if you'd like, @JonathanPlasse!

---
