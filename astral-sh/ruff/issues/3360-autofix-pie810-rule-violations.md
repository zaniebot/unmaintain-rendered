---
number: 3360
title: Autofix PIE810 rule violations
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2023-03-06T15:34:13Z
updated_at: 2023-03-10T05:17:24Z
url: https://github.com/astral-sh/ruff/issues/3360
synced_at: 2026-01-10T01:22:42Z
---

# Autofix PIE810 rule violations

---

_Issue opened by @Skylion007 on 2023-03-06 15:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
PIE810 currently does not have implement any autofixes, but the rule should be readily autofixable using similar logic to SIM101 (which effectively does the same thing with isinstance calls). It would be nice if someone could implement this autofix. This rule is a common minor performance pitfall so having autofixes would help turning this rule on for large codebases.


---

_Label `autofix` added by @charliermarsh on 2023-03-06 16:14_

---

_Label `good first issue` added by @charliermarsh on 2023-03-06 16:14_

---

_Comment by @kyoto7250 on 2023-03-08 14:32_

@charliermarsh 

Hello, I am interested in working on this issue. Could you please assign it to me?

---

_Assigned to @kyoto7250 by @charliermarsh on 2023-03-08 18:17_

---

_Comment by @charliermarsh on 2023-03-08 18:17_

Cool, thanks :)

---

_Referenced in [astral-sh/ruff#3411](../../astral-sh/ruff/pulls/3411.md) on 2023-03-09 01:30_

---

_Closed by @charliermarsh on 2023-03-10 05:17_

---

_Referenced in [ArduPilot/ardupilot#30343](../../ArduPilot/ardupilot/pulls/30343.md) on 2025-06-14 09:04_

---
