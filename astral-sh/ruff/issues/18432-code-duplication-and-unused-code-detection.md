```yaml
number: 18432
title: Code Duplication and Unused Code Detection
type: issue
state: closed
author: ALPA-Industry-and-Technology
labels:
  - question
assignees: []
created_at: 2025-06-02T15:03:55Z
updated_at: 2025-07-24T12:03:13Z
url: https://github.com/astral-sh/ruff/issues/18432
synced_at: 2026-01-10T11:09:58Z
```

# Code Duplication and Unused Code Detection

---

_Issue opened by @ALPA-Industry-and-Technology on 2025-06-02 15:03_

### Question


Hello everyone,

I'm not sure if this is expected behavior or a potential issue, but I’ve come across a situation that I couldn't fully resolve using existing answers on this topic. I hope someone here can help clarify things.

For my company, we’re evaluating various linters and formatters. During our testing, we explored **Ruff** and created a `test_script.py` file containing numerous code smells - specifically, several instances of code duplication (9 lines repeated multiple times within the same file), as well as many unused classes, functions, local variables, and arguments.

We noticed that Ruff, unlike some other static analysis tools, **does not appear to detect code duplications** within the same file. Additionally, while it seems to detect unused local variables and arguments, **unused classes and functions are not flagged** as expected.

Is there a specific reason for this behavior?  
And more importantly, is there a way to configure Ruff (or extend it) to detect **both code duplications** and **unused functions/classes**, across the whole project but also **within the same file**?

Looking forward to your insights.
<br>
Best regards,  
Alex

### Version

ruff 0.11.7

---

_Label `question` added by @ALPA-Industry-and-Technology on 2025-06-02 15:03_

---

_Comment by @MichaReiser on 2025-06-02 15:53_

Hi @ALPA-Industry-and-Technology 

Ruff currently doesn't support duplicate code detection. There are two open issues around it (https://github.com/astral-sh/ruff/issues/17724 and https://github.com/astral-sh/ruff/issues/10522). There's no specific reason that Ruff doesn't support it other than that there have always been more pressing issues to work on.


For unused functions and classes. Python's module system makes detecting unused functions and classes non trivial because all module-level symbols are public. That means, detecting unused functions and classes (and variables) requires searching for all usages across all files. We hope that we'll be able to provide such a lint rule in the future with your new semantic analysis that we build as part of our experimental type checker ty. But it isn't something that ruff supports today because ruff's analysis is single file only. 




---

_Closed by @MichaReiser on 2025-07-24 12:03_

---
