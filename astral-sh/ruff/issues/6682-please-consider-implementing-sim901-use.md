```yaml
number: 6682
title: Please consider implementing SIM901 (Use comparisons directly instead of wrapping them in a bool(...) call )
type: issue
state: open
author: nth10sd
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-18T19:57:52Z
updated_at: 2023-08-20T03:32:36Z
url: https://github.com/astral-sh/ruff/issues/6682
synced_at: 2026-01-10T11:09:48Z
```

# Please consider implementing SIM901 (Use comparisons directly instead of wrapping them in a bool(...) call )

---

_Issue opened by @nth10sd on 2023-08-18 19:57_

It is [an experimental](https://github.com/MartinThoma/flake8-simplify#SIM901) SIM9 flake8-simplify rule and I'm not sure if it's applicable yet.

```
This rule will be renamed to SIM224 after its 6-month trial period is over ...

(will end on 23rd of August 2022.)

# Bad
bool(a == b)

# Good
a == b
```

```
$ ruff --version
ruff 0.0.285
```

---

_Label `rule` added by @dhruvmanila on 2023-08-20 03:32_

---

_Label `needs-decision` added by @dhruvmanila on 2023-08-20 03:32_

---
