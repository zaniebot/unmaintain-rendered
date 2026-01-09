---
number: 9083
title: Scientific notation formatter misses f-strings
type: issue
state: closed
author: claydugo
labels: []
assignees: []
created_at: 2023-12-11T03:21:56Z
updated_at: 2023-12-11T15:25:46Z
url: https://github.com/astral-sh/ruff/issues/9083
synced_at: 2026-01-07T13:12:15-06:00
---

# Scientific notation formatter misses f-strings

---

_Issue opened by @claydugo on 2023-12-11 03:21_

Sorry for not listing the actual rule.
I tried all the flags in `ruff format --help` but never got an output that explains which rule is making the change. 

I can update the title if pointed to which is doing it. Also is there a command I missed that outputs this? `--show-fixes` seems to be only a linter flag.

Reproduce:

```python
int_variable = 3

fstring1 = f"This string contains scientific notation. {int_variable * 1E-3}"
fstring2 = f"string containing scientific notation. {int_variable * 1E3:.2f}"

variable_notation1 = 1E3
variable_notation2 = (int_variable * 1E-3)
```

Output of `ruff format --diff`

```patch
--- example.py
+++ example.py
@@ -3,5 +3,5 @@
 fstring1 = f"This string contains scientific notation. {int_variable * 1E-3}"
 fstring2 = f"string containing scientific notation. {int_variable * 1E3:.2f}"

-variable_notation1 = 1E3
-variable_notation2 = (int_variable * 1E-3)
+variable_notation1 = 1e3
+variable_notation2 = int_variable * 1e-3

1 file would be reformatted
```

---

_Comment by @MichaReiser on 2023-12-11 03:36_

Hy @claydugo 

There's no specific rule that performs this change. Instead, the formatter normalizes the scientific notation to consistently use the lowercase `e.  So, there's no rule that you need to enable or disable. 

However, your observation is correct. The formatter doesn't format expressions inside f-strings yet (similar to black). @dhruvmanila started working on formatting expressions inside of f-strings, which should give you the desired behavior (including splitting expressions to make the f-string fit if possible). 



---

_Renamed from "Scientific Notation formatter misses f-strings" to "Scientific notation formatter misses f-strings" by @claydugo on 2023-12-11 04:00_

---

_Comment by @dhruvmanila on 2023-12-11 15:25_

Hey, thanks for the issue and thanks Micha for providing the context. I'll close this in favor of #7594 which tracks f-string formatting.

---

_Closed by @dhruvmanila on 2023-12-11 15:25_

---
