```yaml
number: 2054
title: Variable reassignment type checking
type: issue
state: closed
author: staciax
labels:
  - question
assignees: []
created_at: 2025-12-18T08:00:05Z
updated_at: 2025-12-18T09:38:47Z
url: https://github.com/astral-sh/ty/issues/2054
synced_at: 2026-01-10T01:53:59Z
```

# Variable reassignment type checking

---

_Issue opened by @staciax on 2025-12-18 08:00_

### Question

`ty` currently allows a variable to be reassigned to incompatible types without reporting any error.

Example:
```py
x = 5
x = "hi" # <--
```

ty output:
```py
All checks passed!
```

mypy output:
```py
test.py:2: error: Incompatible types in assignment (expression has type "str", variable has type "int")  [assignment]
Found 1 error in 1 file (checked 1 source file)
```

**Question**

Once a variable’s type is inferred, should later assignments be checked for compatibility with that inferred type?

If so, is it possible that this could be supported in the future, or made available via a configuration option (for example, a strict or opt-in mode)?

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Label `question` added by @staciax on 2025-12-18 08:00_

---

_Comment by @sharkdp on 2025-12-18 08:20_

Thank you for your question.



> Once a variable’s type is inferred, should later assignments be checked for compatibility with that inferred type?

ty chooses not to do this. There is (in principle) nothing wrong with the code here. If you want `x` to only ever be an int, it needs to be annotated (`x: int = 5`).

> If so, is it possible that this could be supported in the future, or made available via a configuration option (for example, a strict or opt-in mode)?

See [here](https://docs.astral.sh/ty/reference/typing-faq/#does-ty-have-a-strict-mode):

> Not yet. A stricter inference mode is tracked in [this issue](https://github.com/astral-sh/ty/issues/1240). In the meantime, you can consider using Ruff's [flake8-annotations rules](https://docs.astral.sh/ruff/rules/#flake8-annotations-ann) to enforce more explicit type annotations in your code.

---

_Comment by @staciax on 2025-12-18 09:38_

Thank you for the clarification.
That makes sense. I understand that this behavior is intentional and that explicit annotations are the recommended approach.
I’ll follow the stricter inference tracking issue and consider using Ruff’s `flake8-annotations` rules where appropriate.
Thanks for the guidance.

---

_Closed by @staciax on 2025-12-18 09:38_

---
