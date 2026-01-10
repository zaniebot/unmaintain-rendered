```yaml
number: 19577
title: "[Fix error] `SIM905` (split-static-string) autofix does not work with embeded quotes"
type: issue
state: closed
author: tdulcet
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-27T16:50:59Z
updated_at: 2025-08-03T16:31:30Z
url: https://github.com/astral-sh/ruff/issues/19577
synced_at: 2026-01-10T11:09:59Z
```

# [Fix error] `SIM905` (split-static-string) autofix does not work with embeded quotes

---

_Issue opened by @tdulcet on 2025-07-27 16:50_

**Input**:
```py
test_email_addresses = r"""simple@example.com
very.common@example.com
FirstName.LastName@EasierReading.org
x@example.com
long.email-address-with-hyphens@and.subdomains.example.com
user.name+tag+sorting@example.com
name/surname@example.com
xample@s.example
" "@example.org
"john..doe"@example.org
mailhost!username@example.org
"very.(),:;<>[]\".VERY.\"very@\\ \"very\".unusual"@strange.example.com
user%example.com@example.org
user-@example.org
I❤️CHOCOLATE@example.com
this\ still\"not\\allowed@example.com
stellyamburrr985@example.com
Abc.123@example.com
user+mailbox/department=shipping@example.com
!#$%&'*+-/=?^_`.{|}~@example.com
"Abc@def"@example.com
"Fred\ Bloggs"@example.com
"Joe.\\Blow"@example.com""".split("\n")
```
(Yes, these are all technically [valid email addresses](https://en.wikipedia.org/wiki/Email_address#Examples).)

**Output**:
```
$ ruff check --isolated --select SIM905 --fix test.py

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `test.py`, the rule codes SIM905, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```
Version: `ruff 0.12.3`

Playground: https://play.ruff.rs/0d0fc8c2-2f94-4435-ae42-7f4aa46d6cd3

---

_Label `bug` added by @MichaReiser on 2025-07-28 10:02_

---

_Label `fixes` added by @MichaReiser on 2025-07-28 10:02_

---

_Closed by @dylwil3 on 2025-08-03 16:31_

---
