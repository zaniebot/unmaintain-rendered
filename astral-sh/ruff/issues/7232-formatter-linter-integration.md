```yaml
number: 7232
title: Formatter / Linter integration
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - cli
  - formatter
assignees: []
created_at: 2023-09-08T07:42:39Z
updated_at: 2023-09-19T10:04:42Z
url: https://github.com/astral-sh/ruff/issues/7232
synced_at: 2026-01-12T15:54:46Z
```

# Formatter / Linter integration

---

_@MichaReiser_

This depends on the CLI design, e.g. should `ruff check` or `ruff fix` run the linter, and if so, how do you opt-in/opt-out?
 
* Do we want to run the linter and the formatter at the same time? Is the formatter a normal rule code like isort or entirely separate?
* Should activating the formatter deactivate certain lint rules?
* How do we structure fix/format passes?
* When/where do we warn about invalid suppression comment (#6611)
* How to configure the formatter

---

_Assigned to @zanieb by @MichaReiser on 2023-09-08 07:42_

---

_Assigned to @konstin by @MichaReiser on 2023-09-08 07:42_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 07:42_

---

_Renamed from "linter/formatter interaction" to "Formatter / Linter integration" by @MichaReiser on 2023-09-08 07:43_

---

_Label `configuration` added by @MichaReiser on 2023-09-08 07:44_

---

_Label `cli` added by @MichaReiser on 2023-09-08 07:44_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 07:44_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-08 07:44_

---

_Comment by @JonathanPlasse on 2023-09-12 21:35_

I would like Ruff to fix all errors before formatting the code.
The formatting at the end could be enabled/disabled via configuration in a dedicated setting like `ruff.format.run_after_fix`.
This workflow would be similar to my current use of pre-commit where I run Ruff then Black.

---

_Comment by @MichaReiser on 2023-09-15 09:44_

@JonathanPlasse we very much would like that too. But we intentionally exclude integrating the formatter into our fix infrastructure for now not to explode scope. We hope to play around with integrating our formatter once it is stable. 

---

_Closed by @MichaReiser on 2023-09-19 10:04_

---
