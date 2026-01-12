```yaml
number: 19075
title: BLE001 false-positive with Loguru
type: issue
state: open
author: jsleb333
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-01T17:01:21Z
updated_at: 2025-12-29T19:30:32Z
url: https://github.com/astral-sh/ruff/issues/19075
synced_at: 2026-01-12T15:54:56Z
```

# BLE001 false-positive with Loguru

---

_@jsleb333_

### Summary

[BLE001](https://docs.astral.sh/ruff/rules/blind-except/) catches blind exceptions. There is an exception to the rule however, that bare excepts are not flagged when logging.exception is called. Unfortunately, this seems to only work with the built-in logging module. When using the more modern and fairly popular logging library [loguru](https://github.com/Delgan/loguru), the exception is not applied.

It would be nice to support loguru's logging as well in the exception of BLE001.

![Image](https://github.com/user-attachments/assets/a96437ce-e566-460d-b213-3cc5052a8700)


**Ruff version:** 0.12.1

---

_Label `rule` added by @ntBre on 2025-07-01 20:52_

---

_Label `needs-decision` added by @ntBre on 2025-07-01 20:52_

---

_Comment by @ntBre on 2025-07-01 20:54_

There are a few related issues around extending rules to support loguru (https://github.com/astral-sh/ruff/issues/13390, https://github.com/astral-sh/ruff/issues/16715, https://github.com/astral-sh/ruff/issues/13673). I think we've been a bit hesitant to extend support from the standard library to third-party libraries, but I think this definitely makes sense if we decide to support loguru in general!

---

_Comment by @jsleb333 on 2025-07-02 14:21_

Thanks for your answer. I totally get why you'd be hesitant on this topic. There are some rules that applies to third-party libraries, like FastAPI and Airflow, so I'd figure I would propose this rule extension. If modifying BLE is not something that you would consider, maybe adding a loguru rule set would be a better solution.

Regardless whether you add support for loguru or not, your work on Ruff is greatly appreciated! Keep it up! :)

---

_Comment by @ntBre on 2025-12-29 19:30_

I know this was quite a long time ago, but I came across this issue again today and realized that the issues I linked to were a bit misleading. BLE001 respects the [logger-objects](https://docs.astral.sh/ruff/settings/#lint_logger-objects) config setting, so if you add `loguru.logger` then BLE001 should work as expected: https://play.ruff.rs/47dffe4a-2305-414c-9aad-074791de8cfe

For example:

```toml
[lint]
logger-objects = ["loguru.logger"]
```

Thanks for the kind words!

---
