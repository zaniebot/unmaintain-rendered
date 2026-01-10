```yaml
number: 11038
title: Rule E126 implemented?
type: issue
state: closed
author: chuan137
labels:
  - question
assignees: []
created_at: 2024-04-19T13:12:25Z
updated_at: 2024-04-19T13:50:11Z
url: https://github.com/astral-sh/ruff/issues/11038
synced_at: 2026-01-10T11:09:53Z
```

# Rule E126 implemented?

---

_Issue opened by @chuan137 on 2024-04-19 13:12_

my impression is that all the flake8 rules are implemented, but I didn't get [`E126`](https://www.flake8rules.com/rules/E126.html) rule work. tried to enable the rule
```
[tool.ruff.lint]
extend-select = ["E126", "E501"]
```
but got
```
Unknown rule selector: `E126`
```



---

_Comment by @charliermarsh on 2024-04-19 13:50_

It looks like E126 is not implemented. You can see the list here: https://docs.astral.sh/ruff/rules/#error-e. It's tracked here: https://github.com/astral-sh/ruff/issues/2402.

---

_Closed by @charliermarsh on 2024-04-19 13:50_

---

_Label `question` added by @charliermarsh on 2024-04-19 13:50_

---
