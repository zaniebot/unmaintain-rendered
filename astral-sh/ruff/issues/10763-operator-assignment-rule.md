```yaml
number: 10763
title: Operator Assignment Rule
type: issue
state: closed
author: IamRezaMousavi
labels:
  - rule
assignees: []
created_at: 2024-04-03T20:38:16Z
updated_at: 2024-04-03T23:50:13Z
url: https://github.com/astral-sh/ruff/issues/10763
synced_at: 2026-01-12T15:54:50Z
```

# Operator Assignment Rule

---

_@IamRezaMousavi_

Ruff (or other python linters) don't has any rule like [operator-assignment](https://eslint.org/docs/latest/rules/operator-assignment) rule in eslint:

```diff
- x = x + 1
+ x += 1
```


---

_Label `rule` added by @charliermarsh on 2024-04-03 23:25_

---

_Comment by @charliermarsh on 2024-04-03 23:35_

I _think_ this would be covered by https://github.com/astral-sh/ruff/pull/9932

---

_Comment by @IamRezaMousavi on 2024-04-03 23:49_

> I _think_ this would be covered by https://github.com/astral-sh/ruff/pull/9932

Yes, Thank you

---

_Closed by @IamRezaMousavi on 2024-04-03 23:50_

---
