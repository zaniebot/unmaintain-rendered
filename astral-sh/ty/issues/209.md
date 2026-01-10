```yaml
number: 209
title: "Highlight call-arguments range for `reveal_type`, `static_assert`, …"
type: issue
state: closed
author: sharkdp
labels:
  - diagnostics
assignees: []
created_at: 2025-02-03T08:56:01Z
updated_at: 2025-07-23T15:24:13Z
url: https://github.com/astral-sh/ty/issues/209
synced_at: 2026-01-10T02:06:24Z
```

# Highlight call-arguments range for `reveal_type`, `static_assert`, …

---

_Issue opened by @sharkdp on 2025-02-03 08:56_

### Description

We currently highlight the full call expression for `reveal_type`-diagnostics and `static_assert` diagnostics:
```
info: revealed-type
 --> /home/shark/playground/test.py:3:1
  |
1 | from knot_extensions import static_assert
2 |
3 | reveal_type(2 + 3)
  | ------------------ info: Revealed type is `Literal[5]`
4 |
5 | static_assert((2 + 3) < 4)
  |

error: lint:static-assert-error
 --> /home/shark/playground/test.py:5:1
  |
3 | reveal_type(2 + 3)
4 |
5 | static_assert((2 + 3) < 4)
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^ Static assertion error: argument evaluates to `False`
  |
```

I would like it better if only the argument was highlighted. I wanted to make this change quickly, but it's not completely straightforward and messes with `CallOutcome` (which @AlexWaygood is working on), so I figured I'd create a ticket instead.

This is also somewhat related to multiple-range diagnostics, because for functions like `assert_type(<expr>, <type-expr>)`, we might want to highlight the first argument (`"… has type <actual-type>"`) and then create a separate (informational?) text range  for `<type-expr>` to highlight it as the expected type. In any case, we can certainly do better than this:

```
error: lint:type-assertion-failure
 --> /home/shark/playground/test.py:4:1
  |
2 | from typing import assert_type
3 |
4 | assert_type(2 + 3, str)
  | ^^^^^^^^^^^^^^^^^^^^^^^ Actual type `Literal[5]` is not the same as asserted type `str`
  |
```

---

_Label `diagnostics` added by @AlexWaygood on 2025-02-03 08:57_

---

_Comment by @MichaReiser on 2025-02-03 09:54_

CC: @BurntSushi 

---

_Label `diagnostics` added by @MichaReiser on 2025-05-07 15:22_

---

_Renamed from "[red-knot] Highlight call-arguments range for `reveal_type`, `static_assert`, …" to "Highlight call-arguments range for `reveal_type`, `static_assert`, …" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @AlexWaygood on 2025-05-11 07:23_

The `reveal_type` part of this was done in https://github.com/astral-sh/ruff/pull/17980. The `static_assert` and `assert_type` parts aren't yet done.

---

_Assigned to @oconnor663 by @oconnor663 on 2025-07-18 18:32_

---

_Closed by @oconnor663 on 2025-07-23 15:24_

---
