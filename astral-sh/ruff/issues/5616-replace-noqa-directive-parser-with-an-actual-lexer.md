```yaml
number: 5616
title: "Replace `noqa` directive parser with an actual lexer"
type: issue
state: closed
author: charliermarsh
labels:
  - suppression
assignees: []
created_at: 2023-07-08T16:29:31Z
updated_at: 2023-12-07T14:55:25Z
url: https://github.com/astral-sh/ruff/issues/5616
synced_at: 2026-01-12T15:54:45Z
```

# Replace `noqa` directive parser with an actual lexer

---

_@charliermarsh_

In #5567 and related PRs, I moved away from regular expressions and exact matches to a more token-based approach to `noqa` parsing. There are two codepaths here, one for `# noqa: F401` (inline exemptions) and another for `# ruff: noqa: F401` (whole-file exemptions). We should replace both of these with an actual lexer to lex directives, and then match against the resulting token stream. That would likely simplify the code a lot and allow us to unify the implementations.

---

_Label `noqa` added by @charliermarsh on 2023-07-08 16:29_

---

_Comment by @charliermarsh on 2023-07-08 16:30_

Low-priority. We could in theory though use this same lexer for (e.g.) `# isort: on` directives. We could even use this approach to support autoformatting of directives.

---

_Closed by @charliermarsh on 2023-12-07 14:55_

---
