```yaml
number: 4603
title: Possible overlap between TRY400 and G201
type: issue
state: closed
author: ericbn
labels:
  - question
assignees: []
created_at: 2023-05-23T14:05:51Z
updated_at: 2023-06-02T04:59:46Z
url: https://github.com/astral-sh/ruff/issues/4603
synced_at: 2026-01-12T15:54:44Z
```

# Possible overlap between TRY400 and G201

---

_@ericbn_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
#### test.py
Minimal code snipped that reproduces the bug:
```
import logging

logger = logging.getLogger()
try:
    pass
except Exception:
    logger.error("Error", exc_info=True)
```

#### Ruff output

The command I've invoked: `ruff --isolated --select G,TRY test.py`
```
test.py:7:5: TRY400 Use `logging.exception` instead of `logging.error`
test.py:7:12: G201 Logging `.exception(...)` should be used instead of `.error(..., exc_info=True)`
Found 2 errors.
```

After changing the test.py file to:
```diff
-    logger.error("Error", exc_info=True)
+    logger.error("Error")
```
I get only:
```
test.py:7:5: TRY400 Use `logging.exception` instead of `logging.error`
Found 1 error.
```

Maybe the broader TRY400 rule is enough, in favor of G201. Not sure what's the rationale for overlapping/duplicate rules in ruff.

#### ruff --version
```
ruff 0.0.269
```


---

_Comment by @zanieb on 2023-05-23 17:37_

Hm `logger.exception` has a different meaning than `logger.error` — `logger.error` does not include a traceback unless `exc_info` is set but `logger.exception` does. I think these rules cover different cases.

- `G201`: If you're going to log a traceback, use `.exception` instead of `.error` — this could be an automatic fix because the meaning is retained
- `TRY400`: If you're going to log an error, always log a traceback — this would only be a suggested fix because it changes behavior

---

_Comment by @charliermarsh on 2023-05-24 02:07_

Thank you for the extremely clear issue, appreciated as always. I think these are _slightly_ different, but what we _could_ do is change `TRY400` to avoid flagging `logger.error` calls with `exc_info=True`, since those will include a traceback anyway as per the intent of the rule. (Then, suggesting the use of `logging.exception` over `exc_info=True` would be covered separately by `G201`.) What do you think, @madkinsz?


---

_Label `question` added by @charliermarsh on 2023-05-24 02:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-02 04:30_

---

_Closed by @charliermarsh on 2023-06-02 04:59_

---
