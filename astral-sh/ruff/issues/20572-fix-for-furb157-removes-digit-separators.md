```yaml
number: 20572
title: "Fix for `FURB157` removes digit separators"
type: issue
state: closed
author: ntBre
labels:
  - fixes
assignees: []
created_at: 2025-09-25T13:27:29Z
updated_at: 2025-10-29T08:29:51Z
url: https://github.com/astral-sh/ruff/issues/20572
synced_at: 2026-01-12T15:54:57Z
```

# Fix for `FURB157` removes digit separators

---

_@ntBre_

The fix in latest ruff (v0.13.1) converts `Decimal("1_000")` to `Decimal(1000)` instead of to `Decimal(1_000)`.

It becomes more problematic for bigger values, where thousand formatting really helps to read a number, like `Decimal(15_000_000)`.

 _Originally posted by @m-aciek in [#13807](https://github.com/astral-sh/ruff/issues/13807#issuecomment-3332811804)_

---

_Comment by @ntBre on 2025-09-25 13:41_

@m-aciek I spun this off into a separate issue since it seemed fairly different from #13807 to me but definitely worth trying to fix.

From peeking at the rule implementation it might be possible to reuse the original string version instead of the version with `_` removed. If that doesn't work, we could at least mark the fix as unsafe.



---

_Label `fixes` added by @ntBre on 2025-09-25 13:41_

---

_Closed by @ntBre on 2025-10-28 21:47_

---

_Comment by @m-aciek on 2025-10-29 08:29_

Thank you @danparizher @ntBre!

---
