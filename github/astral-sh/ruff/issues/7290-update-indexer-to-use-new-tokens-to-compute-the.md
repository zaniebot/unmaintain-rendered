---
number: 7290
title: "Update `Indexer` to use new tokens to compute the ranges"
type: issue
state: closed
author: dhruvmanila
labels:
  - core
  - python312
assignees: []
created_at: 2023-09-12T09:42:44Z
updated_at: 2023-09-19T06:26:03Z
url: https://github.com/astral-sh/ruff/issues/7290
synced_at: 2026-01-07T13:12:15-06:00
---

# Update `Indexer` to use new tokens to compute the ranges

---

_Issue opened by @dhruvmanila on 2023-09-12 09:42_

The `Indexer` needs to be updated to use the new f-string tokens to compute the following ranges:
1. F-string ranges
2. Triple-quoted string ranges

The f-string ranges (1) can be computed using the start range of a `FStringStart` token to the end range of a `FStringEnd` token. Here, a stack would probably be required as f-strings can be nested and we need to pick up the correct start value for the current end value. It would be similar to computing the ranges of parentheses pair in `(foo, (bar, baz), another, (start, (nested, done), nope), last)`.

A `FStringStart`/`FStringEnd` token consists of prefixes (`f`, `fr`, etc.) and quotes. Using this information we can detect if a f-string is tripled-quoted or not using either of the following proposed solutions:
1. Using the `Locator`, we can extract out the source for the `FStringStart` token using the token range, extract the quotes using [`leading_quote`](https://github.com/astral-sh/ruff/blob/a4a4616f0a49c6b1ff7da93a71308d897d27d21f/crates/ruff_python_ast/src/str.rs#L158-L172) and use the `.text_len` method to check the number of quotes.
2. We can store additional information in the `FStringStart` token itself in the form of [bitflags](https://github.com/astral-sh/ruff/blob/a4a4616f0a49c6b1ff7da93a71308d897d27d21f/crates/ruff_python_parser/src/lexer/fstring.rs#L5-L21) or boolean values similar to `FStringMiddle`. This can then be used to check if it's a tripled-quoted f-string or not.

---

_Referenced in [astral-sh/ruff#7299](../../astral-sh/ruff/issues/7299.md) on 2023-09-12 09:45_

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:45_

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:45_

---

_Label `rule` removed by @dhruvmanila on 2023-09-12 09:47_

---

_Label `core` added by @dhruvmanila on 2023-09-12 09:47_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-13 02:31_

---

_Referenced in [astral-sh/ruff#7325](../../astral-sh/ruff/pulls/7325.md) on 2023-09-13 04:41_

---

_Closed by @dhruvmanila on 2023-09-19 06:26_

---
