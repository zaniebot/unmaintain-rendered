```yaml
number: 13760
title: "`ruff-format` does not account for `tool.ruff.lint`?"
type: issue
state: closed
author: LecrisUT
labels:
  - question
assignees: []
created_at: 2024-10-15T13:02:28Z
updated_at: 2024-10-15T15:31:26Z
url: https://github.com/astral-sh/ruff/issues/13760
synced_at: 2026-01-12T15:54:53Z
```

# `ruff-format` does not account for `tool.ruff.lint`?

---

_@LecrisUT_

In our project we have set `tool.ruff.lint.ignore` to ignore `D210`. But when we tried to add ruff-format pre-commit, we encountered that the rule was still being applied ([example run](https://github.com/teemtee/tmt/pull/2844#issuecomment-2413366922)). This seems like a functionality bug, can someone investigate. Alternatively is there a rule planned to support the opposite approach, i.e. for one-line docstrings enforce having a single space separation between the `"""` markers?


---

_Label `question` added by @MichaReiser on 2024-10-15 13:07_

---

_Comment by @MichaReiser on 2024-10-15 13:09_

The sections in the `.lint` configuration only apply to the linter but not the formatter. The formatter always trims the whitespace from docstrings for consistent formatting and there's no option to change that behavior because we intentionally limit the formatter options and only support formatting styles with widespread adoption.

---

_Comment by @LecrisUT on 2024-10-15 13:15_

I thought that some lint codes are taken into account when formatting, like `D203` vs `D204`, or at the very least the ones that are explicitly incompatible with each other.

---

_Comment by @MichaReiser on 2024-10-15 13:21_

No. The formatter and linter are separate. You can disable formatting by using `fmt: skip` at the end of a statement to disable the formatting of a specific statement. 

---

_Renamed from "`ruff-format` does not account for `tool.ruff.lint.ignore = D210`?" to "`ruff-format` does not account for `tool.ruff.lint`?" by @LecrisUT on 2024-10-15 15:30_

---

_Comment by @LecrisUT on 2024-10-15 15:31_

I guess I will close the issue for now since it is an intentional design decision

---

_Closed by @LecrisUT on 2024-10-15 15:31_

---
