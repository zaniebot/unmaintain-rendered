```yaml
number: 21144
title: "Recognize charsets in different orders (`FURB156`)"
type: issue
state: closed
author: lypwig
labels:
  - rule
assignees: []
created_at: 2025-10-30T15:23:24Z
updated_at: 2025-11-01T02:08:31Z
url: https://github.com/astral-sh/ruff/issues/21144
synced_at: 2026-01-10T11:10:00Z
```

# Recognize charsets in different orders (`FURB156`)

---

_Issue opened by @lypwig on 2025-10-30 15:23_

### Summary

Instead of:

```py
if c in "abcdefghijklmnopqrstuvwxyz":
    print(c)
```

Use:

```py
from string import ascii_lowercase
if c in ascii_lowercase:
    print(c)
```

And more generally:

- `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ` -> `string.ascii_letters`
- `abcdefghijklmnopqrstuvwxyz` -> `string.ascii_lowercase`
- `ABCDEFGHIJKLMNOPQRSTUVWXYZ` -> `string.ascii_uppercase`
- `0123456789` -> `string.digits`
- `0123456789abcdefABCDEF` -> `string.hexdigits`
- `01234567` -> `string.octdigits`

The main reason is, when using a string literal like `abcdefghijklmopqrstuvwxyz` it's not abvious when a letter is missing (oops, just did it, did you see it?), so it's a potential source of errors.

---

_Comment by @ntBre on 2025-10-30 16:09_

This makes sense to me. Clippy has a similar [`manual_is_ascii_check`](https://rust-lang.github.io/rust-clippy/master/index.html#manual_is_ascii_check) too.

---

_Label `rule` added by @ntBre on 2025-10-30 16:09_

---

_Comment by @ntBre on 2025-10-30 16:10_

Oh, actually we have an existing rule for this in preview: [hardcoded-string-charset (FURB156)](https://docs.astral.sh/ruff/rules/hardcoded-string-charset/#hardcoded-string-charset-furb156).

---

_Label `rule` removed by @ntBre on 2025-10-30 16:10_

---

_Label `question` added by @ntBre on 2025-10-30 16:10_

---

_Comment by @lypwig on 2025-10-31 08:26_

Great!

A suggestion: when using the `in` keyword, the rule could work whatever the letters order.

Ie. these lines are similar*, but only the first one is detected by the linter:

<img width="235" height="64" alt="Image" src="https://github.com/user-attachments/assets/918002ce-6c6e-41c0-94de-90655e8688e4" />

https://play.ruff.rs/36ad710a-cafd-42ba-8dce-6778299bdbe5

*It's not exactly the same because the iteration order differs, but I guess it's a very specific use-case to check in a specific order.

---

_Comment by @ntBre on 2025-10-31 13:04_

Ah that's a good point. There's code in the rule for constructing bitsets of characters, but then we don't convert the input to a set for the comparison. The set field is actually completely unused from what I can tell.

---

_Label `question` removed by @ntBre on 2025-10-31 13:05_

---

_Label `rule` added by @ntBre on 2025-10-31 13:05_

---

_Renamed from "Rule request: avoid boilerplate string litterals." to "Recognize charsets in different orders (`FURB156`)" by @ntBre on 2025-10-31 13:06_

---

_Comment by @ntBre on 2025-11-01 02:08_

I looked into this a bit more and was going to make the set change, but I found https://github.com/astral-sh/ruff/issues/13802 and #14233. It's actually intentional that we only perform the check for that exact order because otherwise the diagnostic and fix is likely to be incorrect if `x` in your example isn't a single character:

```pycon
>>> import string
>>> '1' in '1234567890'
True
>>> '012' in '1234567890'
False
>>> '012' in string.digits
True
```

So I'll close this for now.

---

_Closed by @ntBre on 2025-11-01 02:08_

---
