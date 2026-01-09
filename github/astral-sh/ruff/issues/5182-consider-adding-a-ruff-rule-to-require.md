---
number: 5182
title: Consider adding a ruff rule to require explanations for noqa directives
type: issue
state: open
author: ashanbrown
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-19T14:47:23Z
updated_at: 2024-04-08T23:40:11Z
url: https://github.com/astral-sh/ruff/issues/5182
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider adding a ruff rule to require explanations for noqa directives

---

_Issue opened by @ashanbrown on 2023-06-19 14:47_

A rule that requires `noqa` directives to provide an explanation would encourage more readable code.  Often when looking at such a directive, it's hard to know why the author has made the choice to ignore a rule and such a rule would minimize those cases.  This could be implemented by checking for an additional comment following the directive, so that noqa directives would always look like:

```
# noqa: F401 # some explanation here
```

For reference, this has been implemented for `golang` in `golangci-lint` at https://github.com/golangci/golangci-lint/blob/master/pkg/golinters/nolintlint/README.md



---

_Renamed from "consider adding a ruff rule to require explanations for noqa directives" to "Consider adding a ruff rule to require explanations for noqa directives" by @ashanbrown on 2023-06-19 14:50_

---

_Label `question` added by @charliermarsh on 2023-06-20 20:54_

---

_Label `rule` added by @charliermarsh on 2023-06-20 20:54_

---

_Referenced in [astral-sh/ruff#5227](../../astral-sh/ruff/pulls/5227.md) on 2023-06-20 22:31_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:10_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:10_

---

_Comment by @charliermarsh on 2023-07-10 01:10_

For now this is blocked on the ability to add "pedantic" rules.

---

_Referenced in [astral-sh/ruff#8453](../../astral-sh/ruff/issues/8453.md) on 2023-11-03 11:15_

---

_Referenced in [astral-sh/ruff#8454](../../astral-sh/ruff/issues/8454.md) on 2024-04-08 23:39_

---

_Comment by @Avasam on 2024-04-08 23:40_

eslint equivalent: https://eslint-community.github.io/eslint-plugin-eslint-comments/rules/require-description.html , for reference.

---

_Referenced in [astral-sh/ruff#12992](../../astral-sh/ruff/issues/12992.md) on 2024-08-20 03:51_

---
