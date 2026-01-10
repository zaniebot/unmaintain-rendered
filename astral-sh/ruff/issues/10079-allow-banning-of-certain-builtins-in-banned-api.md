```yaml
number: 10079
title: "Allow banning of certain builtins in \"banned-api\""
type: issue
state: open
author: jonathanslenders
labels: []
assignees: []
created_at: 2024-02-22T13:17:54Z
updated_at: 2024-07-11T04:05:02Z
url: https://github.com/astral-sh/ruff/issues/10079
synced_at: 2026-01-10T11:09:52Z
```

# Allow banning of certain builtins in "banned-api"

---

_Issue opened by @jonathanslenders on 2024-02-22 13:17_

I'd love to ban usage of certain builtins using the "banned-api" (TID251) rule.
Something like this: 

```
[tool.ruff.lint.flake8-tidy-imports.banned-api]
# Don't use delattr/getattr/setattr -> too dynamic!
"__builtins__.delattr".msg = "Don't use delattr/setattr/getattr."
"__builtins__.setattr".msg = "Don't use delattr/setattr/getattr."
"__builtins__.getattr".msg = "Don't use delattr/setattr/getattr."

# Don't use `@property`, because methods are more flexible and allow access to unbound methods.
"__builtins__.property".msg = "Don't use properties in this part of the code."

# Don't print. Basically the same as the `T` rule.
"__builtins__.print".msg = "Don't use print."

"__builtins__.list".msg = "Use list literal instead."
```

---

_Comment by @KotlinIsland on 2024-07-11 04:05_

I would prefer `builtins.eggs` over `__builtins__.eggs`, that's the actual name of the module, and:

>As an implementation detail, most modules have the name `__builtins__` made available as part of their globals. The value of `__builtins__` is normally either this module or the value of this module’s [`__dict__`](https://docs.python.org/3/library/stdtypes.html#object.__dict__) attribute. Since this is an implementation detail, it may not be used by alternate implementations of Python.
\- https://docs.python.org/3/library/builtins.html

---
