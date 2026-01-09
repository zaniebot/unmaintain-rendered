---
number: 5241
title: "new rule - ban all usages of `NoReturn` in favor of `Never`"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-21T06:07:13Z
updated_at: 2023-10-16T04:25:13Z
url: https://github.com/astral-sh/ruff/issues/5241
synced_at: 2026-01-07T13:12:15-06:00
---

# new rule - ban all usages of `NoReturn` in favor of `Never`

---

_Issue opened by @DetachHead on 2023-06-21 06:07_

discussed in https://github.com/astral-sh/ruff/issues/5111#issuecomment-1593892209, which was closed:

> imo having two separate types that do the exact same thing is just confusing and misleading. i've seen people misinterpret `NoReturn` to mean that the function returns `None`. having a separate `Never` type while not treating the old one as deprecated just makes that misinterpretation more likely. i feel like the name `NoReturn` was only picked because nobody considered the fact that there are other use cases for it, which is why i think it should be avoided altogether.
> 
> but i guess it comes down to personal preference, so perhaps a new rule could be created to ban all usages of `NoReturn`?

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:24_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:24_

---

_Comment by @DetachHead on 2023-10-16 04:24_

workaround:
```toml
[tool.ruff]
extend-select = [
    "UP035", # bans typing_extensions.NoReturn
    "TID25" # bans typing.NoReturn (see below)
]

[tool.ruff.flake8-tidy-imports.banned-api]
"typing.NoReturn".msg = "use `typing.Never` instead"
```

---
