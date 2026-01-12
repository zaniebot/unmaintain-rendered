```yaml
number: 13666
title: Surrogate code points are internally represented as U+FFFD REPLACEMENT CHARACTER
type: issue
state: open
author: dscorbett
labels:
  - bug
  - core
  - needs-design
assignees: []
created_at: 2024-10-07T16:31:06Z
updated_at: 2025-03-28T03:16:47Z
url: https://github.com/astral-sh/ruff/issues/13666
synced_at: 2026-01-12T15:54:53Z
```

# Surrogate code points are internally represented as U+FFFD REPLACEMENT CHARACTER

---

_@dscorbett_

Ruff represents Python strings with Rust strings, replacing every surrogate with U+FFFD REPLACEMENT CHARACTER. This leads to false positives and incorrect fixes when the exact code point sequence matters. Affected rules include [`duplicate-value` (B033)](https://docs.astral.sh/ruff/rules/duplicate-value/), [`static-join-to-f-string` (FLY002)](https://docs.astral.sh/ruff/rules/static-join-to-f-string/), [`non-unique-enums` (PIE796)](https://docs.astral.sh/ruff/rules/non-unique-enums/), and [`useless-if-else` (RUF034)](https://docs.astral.sh/ruff/rules/useless-if-else/). Ruff should use a non-lossy representation of Python strings, or at least track when there was a surrogate so it can ignore certain rules for strings with surrogates.

```console
$ ruff --version
ruff 0.6.9
$ cat surrogates.py
# False positives:
from enum import Enum
class E(Enum):
    A = "\ud800"
    B = "\ud801"
"\ud800" if False else "\ud801"

# Incorrect fixes:
print(len({"\ud800", "\ud801"}))
print(hex(ord("\ud800".join(("[", "]"))[1])))
$ python surrogates.py
2
0xd800
$ ruff check --isolated --preview --select B033,FLY002,PIE796,RUF034 surrogates.py --output-format concise
surrogates.py:4:5: PIE796 Enum contains duplicate value: `"�"`
surrogates.py:5:1: RUF034 Useless if-else condition
surrogates.py:8:22: B033 [*] Sets should not contain duplicate item `"�"`
surrogates.py:9:15: FLY002 Consider `"[�]"` instead of string join
Found 4 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$ ruff check --isolated --preview --select B033,FLY002,PIE796,RUF034 surrogates.py --unsafe-fixes --fix-only
Fixed 2 errors.
$ python surrogates.py
1
0xfffd
```

---

_Comment by @MichaReiser on 2024-10-07 19:47_

Hmm. Nice find, but likely very annoying to fix. One option is to use a crate like [bstr](https://docs.rs/bstr/latest/bstr/) that doesn't change the UTF16 surrogate pairs. @BurntSushi as author of the `bstr` crate. Any suggestions on how we best represent Python strings that can contain UTF16 surrogate pairs in Rust?

---

_Comment by @BurntSushi on 2024-10-08 00:46_

If you use `bstr`, then your strings would just be `Vec<u8>` and `&[u8]`. But you'd still need to create those in the first place. Which means you need a function mapping `Python string` to `Vec<u8>`. What is that mapping? Incidentally, the Rust standard library uses WTF-8 for what is essentially this exact use case. WTF-8 defines a _superset_ of UTF-8 that permits encoding surrogates, including unpaired surrogates. This allows it to round-trip Windows file paths, which you can roughly think of as allowed to be arbitrary sequences of 16-bit integers. (Just like Unix file paths are, roughly, arbitrary sequences of 8-bit integers.)

However, I think what might make more sense here is to pick a representation that is more faithful to what you're actually trying to model. In this case, that's Python Unicode strings. My understanding is that the logical model supported by Python strings is that they are sequences of Unicode codepoints. In Rust, that would be `Vec<u32>`. (Notably, not `Vec<char>` because a `char` is a Unicode scalar value, which is a strict subset of Unicode codepoints. i.e., All codepoints except for surrogates.)

The problem with `Vec<u32>` is probably that it is expensive and difficult to work with. So it probably makes sense to use a `String` for what is likely the overwhelmingly common case where the Python string can be encoded as UTF-8. (You'll note that `"\ud800".encode('utf-8')` fails.) And then use `Vec<u32>` for anything else. This in turn probably means defining an abstraction so that callers don't need to care about whether a Python string is a `Vec<u32>` or something else. This abstraction may pose costs and other annoyances. For example, it would be impossible to have a method return a `&str` in all cases.

If you went the WTF-8 route, things don't really get that much easier. It's probably just as annoying, if not more so, but in different ways.

If it were me, the next step I'd do here is write down the set of operations we need to perform on Python strings in ruff. If that set is small and well defined, then solving this problem might not be too annoying.

---

_Label `bug` added by @MichaReiser on 2024-10-14 09:09_

---

_Label `needs-design` added by @MichaReiser on 2024-10-14 09:09_

---

_Label `core` added by @MichaReiser on 2024-10-14 09:09_

---

_Comment by @MichaReiser on 2025-03-27 19:26_

RustPython now uses Ruff's parser and they handle surrogates in https://github.com/RustPython/RustPython/pull/5629

---

_Comment by @youknowone on 2025-03-28 01:35_

In https://github.com/RustPython/RustPython/pull/5629, @coolreader18  found the behavior of rust internal's wtf8 is slightly different to Python's requirements

---

_Comment by @coolreader18 on 2025-03-28 03:16_

Yeah, I'd be happy to publish `rustpython-wtf8` for ruff to use if that'd make sense - hopefully it's self-contained enough that we can keep up with PRs for it and you guys don't need to fork it :P

---
