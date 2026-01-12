```yaml
number: 6306
title: "`extend-ignore` and `extend-unfixable` are not defined in `ruff.schema.json`"
type: issue
state: closed
author: eggplants
labels: []
assignees: []
created_at: 2023-08-03T15:05:49Z
updated_at: 2023-08-03T15:18:23Z
url: https://github.com/astral-sh/ruff/issues/6306
synced_at: 2026-01-12T15:54:46Z
```

# `extend-ignore` and `extend-unfixable` are not defined in `ruff.schema.json`

---

_@eggplants_

In ruff playground's setting editor, `extend-ignore` and `extend-unfixable` are reported as `Property {} is not allowed.`.

![image](https://github.com/astral-sh/ruff/assets/42153744/e42d12b9-589e-4157-bf26-8e403004f490)

And I confirm the two property are not defined in `ruff.schema.json`. Is it intentional?

---

_Comment by @charliermarsh on 2023-08-03 15:15_

I believe this is because we intentionally omit those from the JSON schema -- they "work" for convenience but are deprecated in favor of using `ignore` and `unfixable` directly.

---

_Comment by @charliermarsh on 2023-08-03 15:18_

This is a bit confusing but I think it's better than adding these which would in turn autocomplete them (which we don't necessarily want).

---

_Closed by @charliermarsh on 2023-08-03 15:18_

---
