---
number: 19015
title: "[playground] noqa range in playground disagrees with CLI and server for multiline strings"
type: issue
state: closed
author: dylwil3
labels:
  - bug
  - playground
  - suppression
assignees: []
created_at: 2025-06-28T18:15:19Z
updated_at: 2025-09-17T07:29:41Z
url: https://github.com/astral-sh/ruff/issues/19015
synced_at: 2026-01-07T13:12:16-06:00
---

# [playground] noqa range in playground disagrees with CLI and server for multiline strings

---

_Issue opened by @dylwil3 on 2025-06-28 18:15_

### Summary

Compare [this playground example](https://play.ruff.rs/0aa2c315-40df-4149-b581-e07176075967) with the same text

```python
f"""
hey
""" # noqa
```

in either the CLI or an editor (I tested Helix and VSCode). The playground emits an error, but the CLI and editors pick up the noqa.

### Version

_No response_

---

_Label `bug` added by @dylwil3 on 2025-06-28 18:15_

---

_Label `playground` added by @dylwil3 on 2025-06-28 18:15_

---

_Label `suppression` added by @AlexWaygood on 2025-06-28 18:38_

---

_Comment by @TaKO8Ki on 2025-09-16 19:51_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-16 19:54_

---

_Referenced in [astral-sh/ruff#20442](../../astral-sh/ruff/pulls/20442.md) on 2025-09-16 20:12_

---

_Closed by @MichaReiser on 2025-09-17 07:29_

---
