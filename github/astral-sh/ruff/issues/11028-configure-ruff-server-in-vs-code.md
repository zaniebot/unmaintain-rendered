---
number: 11028
title: "Configure `ruff server` in VS Code"
type: issue
state: closed
author: xshapira
labels:
  - question
assignees: []
created_at: 2024-04-19T06:19:26Z
updated_at: 2024-04-19T08:23:46Z
url: https://github.com/astral-sh/ruff/issues/11028
synced_at: 2026-01-07T13:12:15-06:00
---

# Configure `ruff server` in VS Code

---

_Issue opened by @xshapira on 2024-04-19 06:19_

Can we replace Pylance with `ruff server` in VS Code?

I'm not sure if it's possible, but I tried this:
```json
{
// Python
  "python.languageServer": "ruff",
}
```


---

_Comment by @MichaReiser on 2024-04-19 07:34_

I'm not familiar with the setting but I don't think it makes sense to replace Pylance with Ruff today. Ruff's LSP only supports linting and formatting today whereas Pylance supports semantic highlighting, go to definition, rename, etc. 

Maybe in the future ;)

---

_Label `question` added by @MichaReiser on 2024-04-19 07:34_

---

_Comment by @dhruvmanila on 2024-04-19 07:45_

No, that's not possible. For reference, the setting comes from the [VS Code Python extension](https://github.com/microsoft/vscode-python) for which the documentation exists [here](https://code.visualstudio.com/docs/python/settings-reference#_code-analysis-settings).

---

_Comment by @xshapira on 2024-04-19 08:23_

Thanks guys! Hope to see ruff evolving to the point where it'll replace pylance and mypy. 

The ruff team has been crushing it lately. You're doing great!

---

_Closed by @xshapira on 2024-04-19 08:23_

---
