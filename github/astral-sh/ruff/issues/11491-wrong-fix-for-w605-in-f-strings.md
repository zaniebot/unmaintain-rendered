---
number: 11491
title: Wrong fix for W605 in f-strings
type: issue
state: open
author: FreeHarry
labels:
  - bug
assignees: []
created_at: 2024-05-22T07:27:31Z
updated_at: 2024-12-04T13:02:10Z
url: https://github.com/astral-sh/ruff/issues/11491
synced_at: 2026-01-07T13:12:15-06:00
---

# Wrong fix for W605 in f-strings

---

_Issue opened by @FreeHarry on 2024-05-22 07:27_

Python: 3.12.3
Version: ruff 0.4.4
Command: `ruff check --select W605 .\test.py --fix`

And let's given the following code snippet in `test.py`:

```
total = 10
ok = 7
incomplete = 3
s = f"TOTAL: {total}\nOK: {ok}\INCOMPLETE: {incomplete}\n"
```

It is wrongly fixed as:

```
total = 10
ok = 7
incomplete = 3
s = rf"TOTAL: {total}\nOK: {ok}\INCOMPLETE: {incomplete}\n"
```

Now the string is a raw string. The newlines will then interpreted as literals and will not remain newlines.

---

If I do the same for a normal string:

```
s = "TOTAL: 10\nOK: 3\INCOMPLETE: 7\n"
```

It is correctly fixed as:

```
s = "TOTAL: 10\nOK: 3\\INCOMPLETE: 7\n"
```

The invalid escape sequence '\I' is correctly fixed as `\\`, and the newlines are still there. 

---

Even if it is a f-string, but without placeholders, it is done correctly:

```
s = f"TOTAL: 10\nOK: 3\INCOMPLETE: 7\n"
```

Result is correct:

```
s = f"TOTAL: 10\nOK: 3\\INCOMPLETE: 7\n"
```

---

_Label `bug` added by @dhruvmanila on 2024-05-22 10:49_

---

_Comment by @dhruvmanila on 2024-05-22 10:54_

Thank you for the report!

This is happening because each f-string element is analyzed individually. So, first `TOTAL: ` is analyzed, then `\nOK: ` and then `\INCOMPLETE: `, but the checker isn't aware that one of the element contains a newline character.

---

_Referenced in [astral-sh/ruff#14748](../../astral-sh/ruff/pulls/14748.md) on 2024-12-03 05:54_

---

_Comment by @dylwil3 on 2024-12-04 13:02_

Update: The issue in the original post has been resolved, but I'm leaving this open because invalid escapes in format specifiers are not handled yet (see [https://github.com/astral-sh/ruff/pull/14748#issuecomment-2517279287](https://github.com/astral-sh/ruff/pull/14748#issuecomment-2517279287) and the surrounding discussion).

---

_Referenced in [astral-sh/ruff#19543](../../astral-sh/ruff/issues/19543.md) on 2025-07-24 23:18_

---

_Referenced in [astral-sh/ruff#19546](../../astral-sh/ruff/pulls/19546.md) on 2025-07-25 01:34_

---
