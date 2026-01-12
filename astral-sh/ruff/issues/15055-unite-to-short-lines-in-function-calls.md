```yaml
number: 15055
title: Unite to short lines in function calls?
type: issue
state: closed
author: artsiomkaltovich
labels:
  - question
assignees: []
created_at: 2024-12-19T09:30:21Z
updated_at: 2024-12-19T14:52:23Z
url: https://github.com/astral-sh/ruff/issues/15055
synced_at: 2026-01-12T15:54:54Z
```

# Unite to short lines in function calls?

---

_@artsiomkaltovich_

Here’s a polished version of your text:  

---

Apologies if this has already been discussed, but I couldn’t find any issue addressing this case.  

Would it make sense to unify code like this:  

```python
f(1, 
  2, 
  3)
```  

and produce the following result instead?  

```python
f(1, 2, 3)
```  

Currently, this code doesn’t produce any warnings: [Example](https://play.ruff.rs/be74ce4c-5643-43b8-a75b-4abcc1d4a498)

---

_Label `question` added by @MichaReiser on 2024-12-19 12:08_

---

_Comment by @MichaReiser on 2024-12-19 12:09_

You can use `ruff format` to format your code. You can see the formatted code in the [right panel](https://play.ruff.rs/be74ce4c-5643-43b8-a75b-4abcc1d4a498?secondary=Format)

---

_Comment by @artsiomkaltovich on 2024-12-19 13:54_

Hello @MichaReiser Where can I find the command executed for format? Unfortunately, it doesn't work on my project.

---

_Comment by @MichaReiser on 2024-12-19 13:55_

See https://docs.astral.sh/ruff/formatter/

---

_Comment by @artsiomkaltovich on 2024-12-19 14:52_

Thank you for your help

---

_Closed by @artsiomkaltovich on 2024-12-19 14:52_

---
