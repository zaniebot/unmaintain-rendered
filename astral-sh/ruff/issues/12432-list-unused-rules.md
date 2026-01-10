```yaml
number: 12432
title: List unused rules
type: issue
state: open
author: valentincalomme
labels:
  - cli
  - needs-decision
  - needs-design
assignees: []
created_at: 2024-07-21T16:03:38Z
updated_at: 2025-01-15T07:42:02Z
url: https://github.com/astral-sh/ruff/issues/12432
synced_at: 2026-01-10T11:09:54Z
```

# List unused rules

---

_Issue opened by @valentincalomme on 2024-07-21 16:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I love `ruff` because it includes so many rules, many of which I had never heard of before. As the tool adds new rules, I would love to be able to run something like:

`ruff rules --list` and see:
- enabled rules
- disabled rules
- unused rules

This method could exclude unstable rules, rules with auto-fix, etc.

I believe this would help users apply new rules faster and improve documentation. I could list all rules that apply to the project in a script to update my documentation.

---

_Label `cli` added by @charliermarsh on 2024-07-21 17:39_

---

_Label `needs-design` added by @charliermarsh on 2024-07-21 17:39_

---

_Comment by @y2kbugger on 2024-12-04 15:18_

Right now it seems the only way to evolve with new rules is enable ALL and then blacklist everything you don't currently want. Something to see what new rules are available would be nice, this would give the developer control over *when* they want to take the time to triage new rules and not have it come when merely updating dependencies.

---

_Label `needs-decision` added by @MichaReiser on 2025-01-15 07:42_

---
