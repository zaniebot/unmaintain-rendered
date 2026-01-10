```yaml
number: 1201
title: "`datetime.datetime.now()` type is revealed as `Unknown`"
type: issue
state: closed
author: gjcarneiro
labels: []
assignees: []
created_at: 2025-09-18T09:51:54Z
updated_at: 2025-09-18T09:54:20Z
url: https://github.com/astral-sh/ty/issues/1201
synced_at: 2026-01-10T02:06:25Z
```

# `datetime.datetime.now()` type is revealed as `Unknown`

---

_Issue opened by @gjcarneiro on 2025-09-18 09:51_

### Summary

```py
import datetime

def main() -> None:
    xxx = datetime.datetime.now()
    reveal_type(xxx)
```

>    Revealed type: `Unknown` (revealed-type) [Ln 5, Col 17]

https://play.ty.dev/e130475a-e997-41b9-867f-4a179ec65033


The type should be `datetime.datetime`, obviously.

### Version

_No response_

---

_Comment by @sharkdp on 2025-09-18 09:53_

Thank you for reporting this. This will hopefully be solved soon, see #159 (`datetime.now()` returns `Self`) and the ongoing work in https://github.com/astral-sh/ruff/pull/18007

---

_Closed by @sharkdp on 2025-09-18 09:53_

---
