```yaml
number: 3892
title: "Support the `banned-import-alias`"
type: issue
state: closed
author: stancld
labels:
  - rule
assignees: []
created_at: 2023-04-05T22:36:48Z
updated_at: 2023-04-12T03:29:26Z
url: https://github.com/astral-sh/ruff/issues/3892
synced_at: 2026-01-10T11:09:46Z
```

# Support the `banned-import-alias`

---

_Issue opened by @stancld on 2023-04-05 22:36_

## 🚀  Feature request

Currently, `ruff` supports enforcing importing packages/modules under aliases. It would be, however, nice to have a feature to forbid some aliases which might be considered non-standard at some companies.

For example, with a hypothetical configuration
```
[tool.ruff.flake8-import-conventions.forbidden-aliases]
"tensorflow.keras.backend" = "K"
```
the following import
```
>>> import tensorflow.keras.backend as K
```

should raise an error.

Thanks a lot for all your hard work. Ruff is an amazing tool ❤️ 

---

_Renamed from "Support the other variant of `unconventional-import-alias `" to "Support the other variant of `unconventional-import-alias`" by @stancld on 2023-04-05 22:38_

---

_Label `rule` added by @charliermarsh on 2023-04-05 23:41_

---

_Comment by @charliermarsh on 2023-04-05 23:41_

Ooh, I almost replied saying we support this via [`aliases`](https://beta.ruff.rs/docs/settings/#flake8-import-conventions), but you're talking about the inverse: _ban_ certain aliases. That makes sense to me! (And thank you for the kind words :))

---

_Renamed from "Support the other variant of `unconventional-import-alias`" to "Support the `banned-import-alias`" by @stancld on 2023-04-10 11:50_

---

_Closed by @charliermarsh on 2023-04-12 03:29_

---
