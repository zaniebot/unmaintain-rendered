---
number: 16180
title: "F821 Undefined name ignore requires-python >=3.13 in pyproject.toml"
type: issue
state: closed
author: BigOrangeQWQ
labels:
  - question
  - configuration
assignees: []
created_at: 2025-02-16T10:11:58Z
updated_at: 2025-02-16T10:41:34Z
url: https://github.com/astral-sh/ruff/issues/16180
synced_at: 2026-01-10T01:22:57Z
---

# F821 Undefined name ignore requires-python >=3.13 in pyproject.toml

---

_Issue opened by @BigOrangeQWQ on 2025-02-16 10:11_

### Description

 minimal code
```python
def f():
    excs = [OSError("error 1"), SystemError("error 2")]
    raise ExceptionGroup("there were problems", excs)
f()
```

ruff version
```
ruff 0.9.6 (524cf6e51 2025-02-10)
```

ruff check log
```
./ruff.exe check .\minimal.py 
minimal.py:3:11: F821 Undefined name `ExceptionGroup`. Consider specifying `requires-python = ">= 3.11"` or `tool.ruff.target-version = "py311"` in your `pyproject.toml` file.
  |
1 | def f():
2 |     excs = [OSError("error 1"), SystemError("error 2")]
3 |     raise ExceptionGroup("there were problems", excs)
  |           ^^^^^^^^^^^^^^ F821
  |

Found 1 error.
```

pyproject
```
[project]
name = "test"
version = "0.1.0"
description = "test"
requires-python = ">=3.13"
authors = [{ name = "test", email = "test" }]
readme = "README.md"
license = { text = "MIT" }

# [tool.ruff]
# target-version = "py313"
```
After uncommenting the target-version configuration in [tool.ruff] section of my pyproject.toml file, the false positive F821 errors no longer occur.



---

_Label `question` added by @MichaReiser on 2025-02-16 10:27_

---

_Comment by @MichaReiser on 2025-02-16 10:28_

Hi

Can you try keeping the `[tool.ruff]` section? I know, this is confusing but Ruff currently ignores `pyproject.toml` files without a `[tool.ruff]` section. This has come up before and I think we should really fix it

```toml
[project]
name = "test"
version = "0.1.0"
description = "test"
requires-python = ">=3.13"
authors = [{ name = "test", email = "test" }]
readme = "README.md"
license = { text = "MIT" }

[tool.ruff]
```

Related to

https://github.com/astral-sh/ruff/issues/16105
https://github.com/astral-sh/ruff/issues/14813

---

_Label `configuration` added by @MichaReiser on 2025-02-16 10:28_

---

_Comment by @BigOrangeQWQ on 2025-02-16 10:41_

Thank you for your response. I'll keep the [tool.ruff] section and look forward to future fixes.

---

_Closed by @BigOrangeQWQ on 2025-02-16 10:41_

---
