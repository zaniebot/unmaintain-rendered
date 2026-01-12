```yaml
number: 16831
title: ruff check error output with ascii only characters
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-03-18T14:52:34Z
updated_at: 2025-03-19T09:07:38Z
url: https://github.com/astral-sh/ruff/issues/16831
synced_at: 2026-01-12T15:54:55Z
```

# ruff check error output with ascii only characters

---

_@kaddkaka_

### Question

The ruff check error output in ruff v.0.7.0 contains a non-ascii character, a small x (MULTIPLICATION SIGN). Is it possible to turn this into a regular ascii `x`?

```
Fixed 1 error:
- banana.py:
    1 x D209 (new-line...
```

The x in the last line is was I'm referring to.

### Version

ruff 0.7.0

---

_Label `question` added by @kaddkaka on 2025-03-18 14:52_

---

_Comment by @MichaReiser on 2025-03-18 15:01_

> The ruff check error output in ruff v.0.7.0 contains a non-ascii character, a small x (MULTIPLICATION SIGN). Is it possible to turn this into a regular ascii x?

Is the multiplication sign causing you any issues?

---

_Comment by @kaddkaka on 2025-03-18 16:02_

Minor. Because of https://github.com/astral-sh/ruff/issues/14452 we have a python script to parse ruff output
Causing us to hit a ruff lint warning about non ascii in the python code. 

---

_Comment by @MichaReiser on 2025-03-19 09:07_

I prefer the Unicode sign. I think it looks better, and this seems like the exact case where using a noqa comment in your code is the right solution: Yes, you use a Unicode character, but there's a reason for it (it's not accidental). 

---

_Closed by @MichaReiser on 2025-03-19 09:07_

---
