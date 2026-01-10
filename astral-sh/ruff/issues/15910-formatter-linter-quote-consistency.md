---
number: 15910
title: "Formatter/ Linter: Quote consistency"
type: issue
state: closed
author: GutiCW
labels:
  - question
assignees: []
created_at: 2025-02-03T12:33:54Z
updated_at: 2025-02-04T10:00:23Z
url: https://github.com/astral-sh/ruff/issues/15910
synced_at: 2026-01-10T01:22:57Z
---

# Formatter/ Linter: Quote consistency

---

_Issue opened by @GutiCW on 2025-02-03 12:33_

If I configure them as single quotes, I wouldn't expect any string being wrapped in double quotes (simple or tripled).

Wouldn't it be more consistent to always force the according quotes?
E.g., single quotes: If it contains `'` -> format to tripled single quotes `'''`, else simple single quote `'`.

# Keywords
tripled single double quotes Q000 Q001 Q002 Q003

# Code snippet 
`THREE_DOUBLE_QUOTES = """John Doe's text."""`

# Config
```
quote-style = 'single'

[lint.flake8-quotes]
inline-quotes = 'single'
multiline-quotes = 'single'
```

# Sample
[Playground](https://play.ruff.rs/10e85f60-1ffb-4bdb-ade9-fb130207f468)

# Ruff
v0.9.4

---
Perhaps related:
- #7615

---

_Renamed from "Quote consistency" to "Formatter/ Linter: Quote consistency" by @GutiCW on 2025-02-03 12:34_

---

_Comment by @MichaReiser on 2025-02-03 12:42_

I'm unsure I fully understand the question, especially regarding the formatter consistency. 

The formatter uses `"` if a string contains a single quote (`'`) because it requires less space (2 quotes instead of 6). Triple-quoted strings are also primarily used for docstrings or multiline strings. It's also aligned with what many formatters of other ecosystems do and, therefore, is familiar to many users. 



---

_Label `question` added by @MichaReiser on 2025-02-03 12:42_

---

_Comment by @GutiCW on 2025-02-04 08:18_

Thanks for the quick reply.

Ok, so this is expected behavior - understandable as it's already widely used this way, and perhaps a matter of taste anyways.

Just summarizing my points:
- Multi-line strings: Why not always using the chosen quotes for multi-line strings?
- If the string contains the quote character, format to chosen tripled quotes, else format to chosen simple quotes.

That's what I meant with consistency - the chosen quote would be the only character defining a string, no matter what.

Kind regards.

---

_Comment by @MichaReiser on 2025-02-04 08:47_

Thanks for re-iterating. Yes, you can say this is a matter of taste and Ruff's formatter follows PEP8

> In Python, single-quoted strings and double-quoted strings are the same. This PEP does not make a recommendation for this. Pick a rule and stick to it. When a string contains single or double quote characters, however, use the other one to avoid backslashes in the string. It improves readability.
> [source](https://peps.python.org/pep-0008/#string-quotes)

---

_Closed by @GutiCW on 2025-02-04 10:00_

---
