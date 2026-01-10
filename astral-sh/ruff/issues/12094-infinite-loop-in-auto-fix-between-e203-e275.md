```yaml
number: 12094
title: "Infinite loop in auto-fix between `E203`/`E275`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fixes
  - fuzzer
assignees: []
created_at: 2024-06-28T14:50:47Z
updated_at: 2024-06-28T15:21:36Z
url: https://github.com/astral-sh/ruff/issues/12094
synced_at: 2026-01-10T11:09:54Z
```

# Infinite loop in auto-fix between `E203`/`E275`

---

_Issue opened by @dhruvmanila on 2024-06-28 14:50_

Example code:
```py
pass;
```

The [`E275`](https://docs.astral.sh/ruff/rules/missing-whitespace-after-keyword/) rule tries to add a whitespace after `pass` keyword but the [`E203`](https://docs.astral.sh/ruff/rules/whitespace-before-punctuation/) tries to remove the whitespace before `;`.

---

_Label `bug` added by @dhruvmanila on 2024-06-28 14:50_

---

_Label `fixes` added by @dhruvmanila on 2024-06-28 14:50_

---

_Label `fuzzer` added by @dhruvmanila on 2024-06-28 14:50_

---

_Comment by @dhruvmanila on 2024-06-28 14:52_

Although, if you add in `E703` (useless-semicolon), there won't be an infinite loop because it would remove the semicolon.

---

_Comment by @AlexWaygood on 2024-06-28 14:58_

If we're picking sides, I think E203 is correct on this one and E275 has it wrong

---

_Closed by @dhruvmanila on 2024-06-28 15:21_

---

_Closed by @dhruvmanila on 2024-06-28 15:21_

---
