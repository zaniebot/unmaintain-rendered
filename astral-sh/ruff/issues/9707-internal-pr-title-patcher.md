```yaml
number: 9707
title: Internal PR title patcher
type: issue
state: closed
author: johnslavik
labels:
  - ci
assignees: []
created_at: 2024-01-30T15:45:32Z
updated_at: 2025-08-07T16:15:37Z
url: https://github.com/astral-sh/ruff/issues/9707
synced_at: 2026-01-10T11:09:51Z
```

# Internal PR title patcher

---

_Issue opened by @johnslavik on 2024-01-30 15:45_

Would you be interested in a GitHub Action that checks PRs for which rules they affected and edits their titles to meet the format
"[`origin`] Title (`rule-code`)"?

If yes, feel free to assign me. 🚀 

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Renamed from "Internal" to "Internal PR title patcher" by @johnslavik on 2024-01-30 15:45_

---

_Comment by @zanieb on 2024-01-30 15:48_

Maybe if it is gated to pull requests with the `rule` label? I'm a little hesitant for automation that changes the pull request titles but it would be nice to have automation in general. Maybe it'd be better implemented in our changelog generation code? That is also complicated though.

---

_Label `ci` added by @zanieb on 2024-01-30 15:49_

---

_Comment by @MichaReiser on 2025-08-07 16:15_

I'll close this as I haven't felt a strong need for this and is unlikely something that we'll prioritize. Thanks for the offer and opening an issue first

---

_Closed by @MichaReiser on 2025-08-07 16:15_

---
