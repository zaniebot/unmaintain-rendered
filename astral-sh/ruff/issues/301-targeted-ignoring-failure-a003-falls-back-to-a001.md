```yaml
number: 301
title: "Targeted Ignoring Failure: `A003` falls back to `A001`"
type: issue
state: closed
author: ES-Alexander
labels: []
assignees: []
created_at: 2022-10-03T00:30:07Z
updated_at: 2022-10-03T00:50:59Z
url: https://github.com/astral-sh/ruff/issues/301
synced_at: 2026-01-12T15:54:40Z
```

# Targeted Ignoring Failure: `A003` falls back to `A001`

---

_@ES-Alexander_

My team recently incorporated `ruff` into our CI pipeline, and I'm in the process of fixing some newly raised linting errors in our codebase from the latest `ruff` updates :-)

I would like to be able to ignore `A003: BuiltinAttributeShadowing`, but by doing so all the previous `A003` errors fall back to `A001: BuiltinVariableShadowing`, which I do not want to ignore in the general case. Shadowing by a class attribute is much less likely to cause issues or confusion than variables that are global or locally defined within a function, as most class definitions will not make use of builtins directly, and something like `MyClass.list` is not an ambiguous shadow of `list` in general.

While I can semantically understand that "attribute" is a subset of "variable", it seems to hamstring the useful functionality if ignoring a special case means it just gets treated as the general case. In my mind special cases should be treated as mutually exclusive to general cases for ignoring purposes, instead of the current behaviour where specialisation only provides a more specific error message. I'm not sure if this is a buggy implementation detail of this particular pair of errors, or a consistently applied (and potentially intentional) fallback behaviour throughout `ruff`.

---

_Comment by @charliermarsh on 2022-10-03 00:39_

Taking a look now...

---

_Comment by @charliermarsh on 2022-10-03 00:42_

This is actually just a bug :)

Will merge and cut a new release in a sec.

---

_Closed by @charliermarsh on 2022-10-03 00:43_

---

_Comment by @charliermarsh on 2022-10-03 00:44_

Building now as `v0.0.50`. Thank you for filing, great issue!

---

_Comment by @charliermarsh on 2022-10-03 00:45_

Keep 'em coming :)

---

_Comment by @ES-Alexander on 2022-10-03 00:50_

That was fast - thanks! 😄

Will make sure I report anything else that comes up.

---
