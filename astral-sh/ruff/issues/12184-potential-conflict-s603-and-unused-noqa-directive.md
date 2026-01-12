```yaml
number: 12184
title: "Potential conflict S603 and unused noqa directive in `0.5.0` version"
type: issue
state: closed
author: shaneahmed
labels:
  - question
assignees: []
created_at: 2024-07-04T09:32:56Z
updated_at: 2024-07-04T13:42:31Z
url: https://github.com/astral-sh/ruff/issues/12184
synced_at: 2026-01-12T15:54:51Z
```

# Potential conflict S603 and unused noqa directive in `0.5.0` version

---

_@shaneahmed_

Upgrade to 0.5.0 removes `# noqa: S603` during formatting as an unused noqa directive but then generates S603 error.

For example, in https://github.com/TissueImageAnalytics/tiatoolbox/pull/829/files ruff removes `# noqa: S603` as an unused noqa directive` but then fails due to S603 error.

---

_Label `question` added by @MichaReiser on 2024-07-04 11:39_

---

_Comment by @MichaReiser on 2024-07-04 11:44_

Hi @shaneahmed 

The last release changed (breaking) the range of some flake8-bandit rules ([changelog](https://github.com/astral-sh/ruff/releases/tag/0.5.0), [PR](https://github.com/astral-sh/ruff/pull/10667)). This will mean that existing noqa comments need to be moved. I'm sorry that this is causing you extra work. I believe Ruff can do it automatically for you when using `ruff check --extend-select=RUF100 --fix` to remove the unused noqa comments and then use `ruff check --add-noqa --select=S` to add the noqa comments for the flake8-bandit rules. 

---

_Comment by @shaneahmed on 2024-07-04 13:42_

Many thanks @MichaReiser . It helped!

---

_Closed by @shaneahmed on 2024-07-04 13:42_

---
