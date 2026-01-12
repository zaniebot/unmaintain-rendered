```yaml
number: 8671
title: There are too many line breaks after formatting. What can I do to reduce this?
type: issue
state: closed
author: Zander-1024
labels:
  - question
assignees: []
created_at: 2023-11-14T07:15:12Z
updated_at: 2023-11-14T14:12:52Z
url: https://github.com/astral-sh/ruff/issues/8671
synced_at: 2026-01-12T15:54:48Z
```

# There are too many line breaks after formatting. What can I do to reduce this?

---

_@Zander-1024_

line-length looks not work.
If the code is not very long, I don't think it's necessary to wrap a line.

---

_Comment by @charliermarsh on 2023-11-14 14:12_

I think we'll need more information to be helpful here, such as examples of code before and after formatting.

One guess: you could consider disabling magic trailing comma support via [`skip-magic-trailing-comma`](https://docs.astral.sh/ruff/settings/#format-skip-magic-trailing-comma):

```toml
[tool.ruff.format]
skip-magic-trailing-comma = true
```

---

_Closed by @charliermarsh on 2023-11-14 14:12_

---

_Label `question` added by @charliermarsh on 2023-11-14 14:12_

---
