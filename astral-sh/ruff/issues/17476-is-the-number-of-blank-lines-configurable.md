```yaml
number: 17476
title: Is the number of blank lines configurable?
type: issue
state: closed
author: arjun-menon
labels:
  - question
assignees: []
created_at: 2025-04-19T06:36:56Z
updated_at: 2025-11-05T14:55:35Z
url: https://github.com/astral-sh/ruff/issues/17476
synced_at: 2026-01-12T15:54:55Z
```

# Is the number of blank lines configurable?

---

_@arjun-menon_

### Question

I was looking at the code here:

https://github.com/astral-sh/ruff/blob/da6b68cb58824e52efa2a9ae7ebf3c2cbeacad2d/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs#L29-L32

Can the numbers above be configured (i.e. e.g. via `.ruff.toml`)?

Specifically, I'd like to set `BLANK_LINES_TOP_LEVEL` to `1`.

### Version

ruff 0.11.6 (fcd50a049 2025-04-17)

---

_Label `question` added by @arjun-menon on 2025-04-19 06:36_

---

_Comment by @MichaReiser on 2025-04-22 07:41_

No, this isn't configurable because the rule implements the PEP8 style guide which specifies two, respectively one blank line, see https://peps.python.org/pep-0008/#blank-lines)

---

_Closed by @MichaReiser on 2025-04-22 07:41_

---

_Comment by @JezSonic on 2025-10-14 10:48_

Why would it be that hard to give people an option and leave 2 as default for this value?
People that would love to go mentioned style would go with that by default
People that would like to go with their own preferences would be able to actually use them

That's all

---

_Comment by @arjun-menon on 2025-11-05 14:55_

I've re-raised this request/topic here: #21283  cc: @JezSonic 

---
