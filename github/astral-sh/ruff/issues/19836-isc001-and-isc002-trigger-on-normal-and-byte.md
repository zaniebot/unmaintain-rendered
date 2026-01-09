---
number: 19836
title: ISC001 and ISC002 trigger on normal and byte string concatenation
type: issue
state: open
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-08-08T20:33:46Z
updated_at: 2025-08-11T07:43:24Z
url: https://github.com/astral-sh/ruff/issues/19836
synced_at: 2026-01-07T13:12:16-06:00
---

# ISC001 and ISC002 trigger on normal and byte string concatenation

---

_Issue opened by @MeGaGiGaGon on 2025-08-08 20:33_

### Summary

When a normal and byte string are concatenated, both [single-line-implicit-string-concatenation (ISC001)](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/#single-line-implicit-string-concatenation-isc001) and [multi-line-implicit-string-concatenation (ISC002)](https://docs.astral.sh/ruff/rules/multi-line-implicit-string-concatenation/#multi-line-implicit-string-concatenation-isc002) still trigger, making a duplicate message for the same span as the syntax error [playground](https://play.ruff.rs/dc9b49fc-6ec4-4eb4-b7ba-4a6b6efe3421):
```powershell
PS ~>echo @'
foo = "" b""
bar = ""\
b""
'@ | uvx ruff check --select ISC -
-:1:7: invalid-syntax: Bytes literal cannot be mixed with non-bytes literals
  |
1 | foo = "" b""
  |       ^^^^^^
2 | bar = ""\
3 | b""
  |

-:1:7: ISC001 Implicitly concatenated string literals on one line
  |
1 | foo = "" b""
  |       ^^^^^^ ISC001
2 | bar = ""\
3 | b""
  |
  = help: Combine string literals

-:2:7: invalid-syntax: Bytes literal cannot be mixed with non-bytes literals
  |
1 |   foo = "" b""
2 |   bar = ""\
  |  _______^
3 | | b""
  | |___^
4 |   baz = (""
5 |   + b"")
  |

-:2:7: ISC002 Implicitly concatenated string literals over multiple lines
  |
1 |   foo = "" b""
2 |   bar = ""\
  |  _______^
3 | | b""
  | |___^ ISC002
4 |   baz = (""
5 |   + b"")
  |

Found 4 errors.
```

Both ISC001 and ISC002 shouldn't trigger here to prevent the duplicate message.

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Comment by @ntBre on 2025-08-08 21:42_

I'm kind of on the fence about whether this is a bug or not. It feels like the natural thing to do when seeing the syntax error would be to remove the `b` (or add one), which would then reveal the ISC errors, so I think it makes sense to show them both from the start. But I'm open to other opinions.

---

_Comment by @MeGaGiGaGon on 2025-08-08 21:54_

One other point of consideration: This same thing will also happen when the syntax error for mixing t-strings with non-t-strings is implemented into ruff on 3.14+

---

_Comment by @MichaReiser on 2025-08-11 07:43_

> One other point of consideration: This same thing will also happen when the syntax error for mixing t-strings with non-t-strings is implemented into ruff on 3.14+

This is already the case today: https://play.ruff.rs/63689b22-738f-46fc-961d-6ad329a84d3a

I feel like either approach is fine here: 

* Showing both errors: Gives users the full context and they can directly apply the right fix
* Only showing the invalid syntax error: This reduces the noise in an IDE and makes it easier for users to spot the most important errors (fixing the syntax error is necessary to run the program, fixing the ISC violation can be done later)

---
