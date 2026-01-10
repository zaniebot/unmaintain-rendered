```yaml
number: 18106
title: "Rule Request: change call args to keywords"
type: issue
state: open
author: ilius
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2025-05-14T19:17:26Z
updated_at: 2025-05-15T07:59:33Z
url: https://github.com/astral-sh/ruff/issues/18106
synced_at: 2026-01-10T11:09:58Z
```

# Rule Request: change call args to keywords

---

_Issue opened by @ilius on 2025-05-14 19:17_

When there are many arguments (3+) in a function/class call, using keyword arguments improves readability.
It would be nice to add an option to linter or formatter (min argument count) to change function/class calls to keywords (default `0` = disabled)

---

_Renamed from "Formatter Feature: change arguments to keywords" to "Formatter Feature: change call args to keywords" by @ilius on 2025-05-14 19:20_

---

_Comment by @ntBre on 2025-05-14 20:03_

This sounds interesting! I could be wrong, or too pedantic, but I think this sounds more like a stylistic lint rule than something for the formatter to handle. I think we'll also need type inference/multi-file analysis to perform this kind of fix in the general case since it requires looking up the function definition to get the argument names.

It's also somewhat related to [too-many-positional-arguments (PLR0917)](https://docs.astral.sh/ruff/rules/too-many-positional-arguments/#too-many-positional-arguments-plr0917), which checks for too many positional parameters in the function definition rather than the call.

---

_Label `rule` added by @ntBre on 2025-05-14 20:03_

---

_Label `type-inference` added by @ntBre on 2025-05-14 20:03_

---

_Comment by @ilius on 2025-05-15 06:33_

Thanks for the info.
I also suspected formatter may not have access to function/class signature/declaration when it sees a call.

> It's also somewhat related to [too-many-positional-arguments (PLR0917)](https://docs.astral.sh/ruff/rules/too-many-positional-arguments/#too-many-positional-arguments-plr0917), which checks for too many positional parameters in the function definition rather than the call.

Fixing this (other than breaking the function up) requires adding default values to the arguments, and if the `None` is logical default, changing the type to `| None` and then checking in the body if they are None... Which adds extra complexity to already complex functions. I think "cyclomatic complexity" (C90 rule) is a much better way to spot complex functions.



---

_Renamed from "Formatter Feature: change call args to keywords" to "Rule Request: change call args to keywords" by @ilius on 2025-05-15 06:33_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-15 06:48_

---

_Comment by @MichaReiser on 2025-05-15 06:49_

Marking this as "needs-decision" because it's a very opinionated rule (restriction rule, intentionally disallowing a certain syntax only because of "I prefer it this way")

---

_Comment by @ilius on 2025-05-15 06:58_

Using keyword arguments can prevent bugs and typos specially when adding or removing args, re-ordering args, breaking down functions etc. Plus the readability (to see what these args are without hovering over every call). I think that's why Python started supporting keyword syntax for positional arguments.
It also allows you to `grep -r` a certain argument name with its value in a directory.

Also most lint rules are somewhat opinionated.

---
