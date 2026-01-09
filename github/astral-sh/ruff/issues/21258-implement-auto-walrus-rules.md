---
number: 21258
title: "Implement `auto-walrus` rules?"
type: issue
state: open
author: akx
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-03T15:42:43Z
updated_at: 2025-11-03T20:26:36Z
url: https://github.com/astral-sh/ruff/issues/21258
synced_at: 2026-01-07T13:12:16-06:00
---

# Implement `auto-walrus` rules?

---

_Issue opened by @akx on 2025-11-03 15:42_

### Summary

As discussed over at https://github.com/MarcoGorelli/auto-walrus/pull/85#issuecomment-3479776713, would there be interest to include `auto-walrus`'s functionality, that is

```diff
-    n = 10
-    if n > 3:
+    if (n := 10) > 3:
         print(n)
```

etc. into Ruff?

(Ruff might already have some control flow analysis features that could make it simple to implement the cases that would currently be `--unsafe` for `auto-walrus`.)

---

_Comment by @ntBre on 2025-11-03 18:20_

There's some previous discussion along these lines in https://github.com/astral-sh/ruff/issues/3464. I think it's not clear yet if we want to have a rule adding walrus expressions, removing them, or both, especially since either option can be divisive.

---

_Label `rule` added by @ntBre on 2025-11-03 18:20_

---

_Label `needs-decision` added by @ntBre on 2025-11-03 18:20_

---

_Comment by @akx on 2025-11-03 20:26_

Yeah, I saw #3464, but that's strictly about disallowing walruses. I think there are other rules in Ruff already that are logically mutually exclusive anyway :)

---
