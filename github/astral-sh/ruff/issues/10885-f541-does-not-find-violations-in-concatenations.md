---
number: 10885
title: F541 does not find violations in concatenations
type: issue
state: closed
author: samueljsb
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-04-11T16:00:55Z
updated_at: 2024-04-16T14:33:25Z
url: https://github.com/astral-sh/ruff/issues/10885
synced_at: 2026-01-07T13:12:15-06:00
---

# F541 does not find violations in concatenations

---

_Issue opened by @samueljsb on 2024-04-11 16:00_

`ruff` does not correctly identify f-strings with no placeholders if they are being concatenated with other f-strings that *do* have placeholders. e.g:

```python
placeholder = "placeholder"
(
    f"first line no placeholder"
    f"second line with {placeholder}"
)
(
    f"first line with {placeholder}"
    f"second line with  no placeholder"
)
```

```console
$ ruff check t.py
All checks passed!
```

The error is correctly found if there is no other f-string:

```python
placeholder = "placeholder"
(
    f"first line no placeholder"
    "second line with no placeholder"
)
```

```console
$ ruff check t.py
t.py:3:5: F541 [*] f-string without any placeholders
Found 1 error.
[*] 1 fixable with the `--fix` option.
```


This was demonstrated in a clean venv with `ruff` 0.3.5.


---

_Referenced in [astral-sh/ruff#10886](../../astral-sh/ruff/pulls/10886.md) on 2024-04-11 16:28_

---

_Comment by @samueljsb on 2024-04-11 16:28_

I've opened #10886 to demonstrate this in a test.

---

_Comment by @dhruvmanila on 2024-04-12 05:30_

Thanks! I think it was not possible to detect this reliably before 017e82911 but it should be trivial now.

---

_Label `rule` added by @dhruvmanila on 2024-04-12 05:30_

---

_Label `help wanted` added by @dhruvmanila on 2024-04-12 05:30_

---

_Comment by @samueljsb on 2024-04-16 14:33_

It was agreed in #10886 that this rule, to match the behaviour of `pyflakes`, should not detect this as a violation.

I'm not convinced that there's any deliberate reason for doing that in `pyflakes` (it was brought up in PyCQA/pyflakes#552 but the only discussion was about why the AST is that way rather than why the rule doesn't consider it a problem). But, for compatibility, leaving this rule alone makes sense.

I'll close this issue, since F541 is behaving as expected.

---

_Closed by @samueljsb on 2024-04-16 14:33_

---

_Referenced in [astral-sh/ruff#11357](../../astral-sh/ruff/issues/11357.md) on 2024-05-10 05:17_

---
