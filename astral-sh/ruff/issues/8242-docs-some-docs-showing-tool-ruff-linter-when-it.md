```yaml
number: 8242
title: "Docs: Some docs showing `[tool.ruff.linter]` when it should be `[tool.ruff.lint]`"
type: issue
state: closed
author: thernstig
labels: []
assignees: []
created_at: 2023-10-26T06:09:25Z
updated_at: 2023-10-26T06:52:22Z
url: https://github.com/astral-sh/ruff/issues/8242
synced_at: 2026-01-10T11:09:50Z
```

# Docs: Some docs showing `[tool.ruff.linter]` when it should be `[tool.ruff.lint]`

---

_Issue opened by @thernstig on 2023-10-26 06:09_

Example: https://docs.astral.sh/ruff/tutorial/#configuration

---

_Comment by @MichaReiser on 2023-10-26 06:14_

Thanks for reporting. We landed a few fixes in the last few days but haven't redeployed the documentation yet. 
I quickly searched and couldn't find any remaining `ruff.linter` or `linter` usages. So this should be fixed,  but requires a doc redeployment.

---

_Comment by @thernstig on 2023-10-26 06:51_

@MichaReiser I understand, I thought it was immediate. Apologize for this noise. Closing.

---

_Closed by @thernstig on 2023-10-26 06:51_

---

_Comment by @MichaReiser on 2023-10-26 06:52_

> @MichaReiser I understand, I thought it was immediate. Apologize for this noise. Closing.

No worries. It's extremely helpful to get these reports and they improve our user experience a lot. Keep it up :)

---
