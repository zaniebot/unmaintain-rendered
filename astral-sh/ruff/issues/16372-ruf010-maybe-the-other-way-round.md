```yaml
number: 16372
title: RUF010 Maybe the other way round?
type: issue
state: closed
author: BLKSerene
labels:
  - question
assignees: []
created_at: 2025-02-25T14:54:19Z
updated_at: 2025-02-27T14:03:32Z
url: https://github.com/astral-sh/ruff/issues/16372
synced_at: 2026-01-12T15:54:55Z
```

# RUF010 Maybe the other way round?

---

_@BLKSerene_

### Question

Hi, the doc of [`RUF010`](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/) states that the dedicated conversion flags (`!s`, `!r`, `!a`) are more succinct and idiomatic.

While I could agree that these conversion flags are more succinct than their function call equivalents (`str()`, `repr()`, `ascii()`), I don't think they are idiomatic.

[PEP 498](https://peps.python.org/pep-0498/#s-r-and-a-are-redundant) which introduced f-strings explicitly says that they are redundant. And strictly speaking, these are not dedicated conversion flags introduced by f-strings. Rather, as stated in the PEP, they are introduced mainly for compatibility issues and maybe to smooth the transition from `str.format()` to f-strings.

> However, !s, !r, and !a are supported by this PEP in order to minimize the differences with str.format(). !s, !r, and !a are required in str.format() because it does not allow the execution of arbitrary expressions.

Should str.format() allow the execution of arbitrary expressions, `!s`, `!r`, `!a` would not have been introduced as they would have violated the Zen of Python that

> There should be one-- and preferably only one --obvious way to do it.

IMHO, `RUF010` should convert `!s`, `!r`, `!a` into `str()`, `repr()`, `ascii()` respectively instead of the other way around.

### Version

_No response_

---

_Label `question` added by @BLKSerene on 2025-02-25 14:54_

---

_Renamed from "[RUF010] Maybe the other way round?" to "RUF010 Maybe the other way round?" by @BLKSerene on 2025-02-25 15:55_

---

_Comment by @MichaReiser on 2025-02-25 17:19_

I did a quick search to see if one is more common than the other and it seems both are used a lot:

* [`ascii`, `str`, `repr`](https://sourcegraph.com/search?q=context:global+lang:Python+f%5C%22.*%5C%7B%28str%7Crepr%7Cascii%29%5C%28.%2B%5C%29%5C%7D&patternType=regexp&sm=0)
* [f-string conversion flags](https://sourcegraph.com/search?q=context:global+lang:Python+f%5C%22.*%5C%7B.%2B%5C%21%5Br%7Cs%7Ca%5D%5C%7D&patternType=regexp&sm=0)


@zanieb you asked me to ping you more often. Any thoughts on how idiomatic one or the other is?

---

_Comment by @zanieb on 2025-02-25 23:02_

Personally, I've always used the `!r` flag in f-strings and definitely find it more idiomatic.

As a simple example, I ran some GitHub code searches

- `{...!r}` [214k hits](https://github.com/search?q=%2F%5C%7B%5Ba-z%5D%2B%5C%21r%5C%7D%2F&type=code)
- `{repr(...)}` [50k hits](https://github.com/search?q=%2F%5C%7Brepr%5C%28%5Ba-z%5D%2B%5C%29%5C%7D%2F&type=code)

It is an interesting point that the PEP notes they're redundant, but that doesn't mean they're not more idiomatic.

Given that they're direct corollaries of the `str.format` and `%` style formatting options, it also seems _more_ consistent to use them.

Unless there's clear evidence this is explicitly _not_ idiomatic, I think it should remain as-is.

---

_Comment by @zanieb on 2025-02-26 00:35_

I think a great reference for idiomatic usage is the CPython source code itself, which, from Micha's searches above, has 5 uses of the functional versions (and most look justified by more complex invocations) vs 500+ uses of the conversion flags.

---

_Closed by @BLKSerene on 2025-02-27 14:03_

---
