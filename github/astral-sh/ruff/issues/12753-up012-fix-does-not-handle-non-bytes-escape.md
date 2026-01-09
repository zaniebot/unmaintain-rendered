---
number: 12753
title: UP012 fix does not handle non-bytes escape sequences
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-08-08T13:29:05Z
updated_at: 2024-08-09T05:08:06Z
url: https://github.com/astral-sh/ruff/issues/12753
synced_at: 2026-01-07T13:12:15-06:00
---

# UP012 fix does not handle non-bytes escape sequences

---

_Issue opened by @dscorbett on 2024-08-08 13:29_

When converting string literals to bytes literals, UP012 does not handle escape sequences that are only recognized in string literals. It should convert them to literal characters or to recognized escape sequences.

```console
$ ruff --version
ruff 0.5.6
$ cat up012.py
print("\N{DIGIT ONE}".encode())
print("\u0031".encode())
print("\U00000031".encode())
$ python up012.py 
b'1'
b'1'
b'1'
$ ruff check --isolated --select UP012 up012.py --fix
Found 3 errors (3 fixed, 0 remaining).
$ python up012.py
b'\\N{DIGIT ONE}'
b'\\u0031'
b'\\U00000031'
```

---

_Label `bug` added by @charliermarsh on 2024-08-09 01:26_

---

_Comment by @charliermarsh on 2024-08-09 01:27_

Thanks, seems like a clear bug.

---

_Comment by @charliermarsh on 2024-08-09 01:27_

Is this done via NFKC normalization?

---

_Comment by @dhruvmanila on 2024-08-09 05:08_

I don't think so. I think the problem is that we're using the raw string in the fix (via the tokens), so Ruff would make the following edit:

```diff
-print("\N{DIGIT ONE}".encode())
-print("\u0031".encode())
-print("\U00000031".encode())
+print(b"\N{DIGIT ONE}")
+print(b"\u0031")
+print(b"\U00000031")
```

But, we should instead use the string from the AST as it contains the replaced value (https://play.ruff.rs/f858f470-796a-4d4f-bd51-ef36f291b863). This would make the diff as:

```diff
-print("\N{DIGIT ONE}".encode())
-print("\u0031".encode())
-print("\U00000031".encode())
+print(b"1")
+print(b"1")
+print(b"1")
```

I wonder if this might bring its own set of bugs so it could be better to just ignore bytes with escapes. This is what `pyupgrade` does as well.

---

_Referenced in [astral-sh/ruff#12936](../../astral-sh/ruff/issues/12936.md) on 2024-08-20 15:11_

---

_Referenced in [astral-sh/ruff#16058](../../astral-sh/ruff/pulls/16058.md) on 2025-02-10 00:20_

---
