---
number: 9666
title: False positive of TCH003 on InitVar import
type: issue
state: closed
author: jonyscathe
labels:
  - bug
assignees: []
created_at: 2024-01-29T05:31:30Z
updated_at: 2024-01-29T22:00:18Z
url: https://github.com/astral-sh/ruff/issues/9666
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive of TCH003 on InitVar import

---

_Issue opened by @jonyscathe on 2024-01-29 05:31_

TCH003 should not be raised on InitVar or ClassVar import even if they are only used for typing within a dataclass.
This is because they are needed at runtime.

See the corresponding issue in flake8-type-checking for more information: https://github.com/snok/flake8-type-checking/issues/83

Not sure if it should be fixed in the same way in ruff.


Currently using ruff==0.1.14

Settings:
[tool.ruff]
extend-select = ['D213']
ignore = ['ANN101', 'ANN401', 'D107', 'D203', 'D212', 'E203', 'ISC003', 'N803', 'N806', 'PLC1901', 'PLC2701']
line-length = 120
preview = true
select = ['ALL']
target-version = 'py312'

[tool.ruff.format]
line-ending = 'lf'
quote-style = 'single'

[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false

[tool.ruff.lint.flake8-quotes]
inline-quotes = 'single'

[tool.ruff.lint.flake8-type-checking]
strict = true

[tool.ruff.lint.isort]
known-first-party = ['namespace.project']

[tool.ruff.lint.pylint]
max-args = 8


---

_Comment by @jonyscathe on 2024-01-29 05:52_

I now realise that the setting: `exempt-modules = ["dataclasses"]` does fix this.  
So I guess that is fine. Not the most ideal solution as it could mask genuine issues, but it'll work.

---

_Comment by @charliermarsh on 2024-01-29 11:56_

Is the right solution here not to add `@dataclass` to `runtime-evaluated-decorators`? See: https://docs.astral.sh/ruff/settings/#flake8-type-checking-runtime-evaluated-decorators.

---

_Comment by @charliermarsh on 2024-01-29 11:57_

Oh, but I guess this only applies to `InitVar` specifically, and not other annotations used within dataclasses?

---

_Label `question` added by @charliermarsh on 2024-01-29 11:57_

---

_Label `question` removed by @charliermarsh on 2024-01-29 17:13_

---

_Label `bug` added by @charliermarsh on 2024-01-29 17:13_

---

_Comment by @charliermarsh on 2024-01-29 17:13_

We can fix this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-29 20:09_

---

_Referenced in [astral-sh/ruff#9688](../../astral-sh/ruff/pulls/9688.md) on 2024-01-29 20:36_

---

_Closed by @charliermarsh on 2024-01-29 21:27_

---

_Comment by @jonyscathe on 2024-01-29 22:00_

Thanks for the quick fix!
It is annoying that there are a handful of special cases like this in Python where the typing is actually used at runtime.

---
