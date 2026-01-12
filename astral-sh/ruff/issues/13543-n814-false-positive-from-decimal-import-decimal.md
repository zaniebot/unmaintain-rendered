```yaml
number: 13543
title: "N814 false positive: `from decimal import Decimal as D`"
type: issue
state: closed
author: rdaysky
labels:
  - rule
  - needs-info
assignees: []
created_at: 2024-09-28T03:39:16Z
updated_at: 2024-11-20T14:51:35Z
url: https://github.com/astral-sh/ruff/issues/13543
synced_at: 2026-01-12T15:54:53Z
```

# N814 false positive: `from decimal import Decimal as D`

---

_@rdaysky_

N814 ([camelcase-imported-as-constant](https://docs.astral.sh/ruff/rules/camelcase-imported-as-constant/)) triggers even if the alias is a single uppercase letter in [code like](https://docs.python.org/dev/library/statistics.html#statistics.mean)
```
from decimal import Decimal as D
from fractions import Fraction as F
```
but perhaps it shouldn’t?

---

_Label `bug` added by @charliermarsh on 2024-09-28 12:42_

---

_Comment by @charliermarsh on 2024-09-28 12:42_

I tend to agree that we shouldn't flag this.

---

_Label `bug` removed by @charliermarsh on 2024-09-28 12:43_

---

_Label `rule` added by @charliermarsh on 2024-09-28 12:43_

---

_Label `help wanted` added by @charliermarsh on 2024-09-28 12:43_

---

_Comment by @rdaysky on 2024-09-28 17:32_

It’s also an interesting question whether `from collections.abc import AsyncGenerator as AG` is acceptable style for a module that defines a lot of them.

---

_Comment by @dhruvmanila on 2024-10-01 03:59_

Did you mean `N817` instead of `N814`? These won't be flagged by the latter as it'll be considered as an acronym for the imported name. See https://play.ruff.rs/9326f4aa-a8bf-47c0-a45f-5b3d04b902c7.

---

_Comment by @dhruvmanila on 2024-10-01 04:03_

You can also use the [`extend-aliases`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_extend-aliases) option here:

```toml
[tool.ruff.lint.flake8-import-conventions.extend-aliases]
"collections.abc.AsyncGenerator" = "AG"
"decimal.Decimal" = "D"
"fractions.Fraction" = "F"
```

---

_Comment by @dhruvmanila on 2024-10-03 15:14_

I think I'd suggest to use the configuration instead of adding special casing for certain imports. What do you think? @rdaysky 

---

_Label `help wanted` removed by @MichaReiser on 2024-10-07 07:37_

---

_Label `rule` removed by @MichaReiser on 2024-10-07 07:37_

---

_Label `needs-info` added by @MichaReiser on 2024-10-07 07:37_

---

_Label `rule` added by @MichaReiser on 2024-10-07 07:37_

---

_Closed by @MichaReiser on 2024-11-20 14:51_

---
