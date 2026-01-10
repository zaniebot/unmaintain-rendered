```yaml
number: 130
title: "unused suppression for `type: ignore`"
type: issue
state: closed
author: MichaReiser
labels:
  - suppression
assignees: []
created_at: 2025-04-10T16:39:45Z
updated_at: 2025-12-31T15:21:54Z
url: https://github.com/astral-sh/ty/issues/130
synced_at: 2026-01-10T01:56:40Z
```

# unused suppression for `type: ignore`

---

_Issue opened by @MichaReiser on 2025-04-10 16:39_

https://github.com/astral-sh/ruff/pull/17305 revealed an interesting case where Red Knot raised an unused suppression diagnostic when the user intended to suppress it:

```py
text_encoding = (
    io.text_encoding  # type: ignore[unused-ignore, attr-defined]
    if sys.version_info > (3, 10)
    else _text_encoding
)
```

Source: https://github.com/jaraco/zipp/blob/main/zipp/compat/py310.py

`type: ignore` currently acts as a wildcard suppression (we disregard anything coming in `[...]`) and suppresses anything **except** unused suppression issues. The result is that users aren't able to suppress unused suppression diagnostics with `type: ignore` and are forced to use a red knot specific suppression instead. 

Different options are:

* Change `type: ignore` to suppress all diagnostics (including unused suppression)
* Accept that users have to use a red knot specific suppression for unused suppression diagnostics (this is what Ruff does)
* Add support for `type: ignore[unused-ignore]` 
* ..?


---

_Renamed from "[red-knot] unused suppression for `type: ignore`" to "unused suppression for `type: ignore`" by @MichaReiser on 2025-05-07 15:25_

---

_Label `suppression` added by @AlexWaygood on 2025-05-10 18:07_

---

_Comment by @MichaReiser on 2025-12-24 14:26_

I think asking users to use `ty: ignore[unused-ignore-comment]` seems reasonable. 

An alternative to this is to introduce a separate rule for unused `type: ignore` comments, which a user could disable.

---

_Closed by @MichaReiser on 2025-12-31 15:21_

---
