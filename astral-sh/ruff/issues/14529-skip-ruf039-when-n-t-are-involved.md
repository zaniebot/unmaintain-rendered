```yaml
number: 14529
title: Skip RUF039 when \n, \t, ... are involved
type: issue
state: closed
author: alexprengere
labels:
  - question
assignees: []
created_at: 2024-11-22T13:26:53Z
updated_at: 2024-11-22T15:52:07Z
url: https://github.com/astral-sh/ruff/issues/14529
synced_at: 2026-01-12T15:54:53Z
```

# Skip RUF039 when \n, \t, ... are involved

---

_@alexprengere_

The latest Ruff 039 rule is a bit noisy (see #14527 too), and I think there are false positives: running it on a private codebase, I had a lot of:

```
_RE_STRIP = re.compile("[ \t\r\n]+")
                        ^^^^^^^^^^ RUF039
= help: Replace with raw string
```
Unless I missed something, there is no *easy* way to convert that one to a raw string, as `r"\n"` is semantically different to `"\n"` (but I agree that is a good general rule, when possible).

---

_Label `question` added by @MichaReiser on 2024-11-22 14:25_

---

_Comment by @MichaReiser on 2024-11-22 14:30_

Converting to a raw string should be possible for all escape sequence except `\b`

![image](https://github.com/user-attachments/assets/0f6e2848-0156-4372-8ff7-d15c6a2c2450)
[source](https://docs.python.org/3/library/re.html#re.Pattern)

You can also verify this using the repl

```python
>>> re.match("\n", "\n")
<re.Match object; span=(0, 1), match='\n'>
>>> re.match(r"\n", "\n")
<re.Match object; span=(0, 1), match='\n'>
>>> 
```

---

_Comment by @alexprengere on 2024-11-22 15:50_

Thanks for the reply! I guess CPython implemented this to make people's life easier with the raw strings.
It *feels* a bit weird as it blurs some lines between what is escaped by `re` and what is escaped when building a string, but I guess as this works, I will close the issue. 

---

_Closed by @MichaReiser on 2024-11-22 15:52_

---
