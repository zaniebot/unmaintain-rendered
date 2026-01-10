```yaml
number: 2100
title: "ty not working with Marimo's VSCode extension"
type: issue
state: open
author: hierr
labels:
  - notebook
assignees: []
created_at: 2025-12-19T05:56:31Z
updated_at: 2025-12-23T11:36:10Z
url: https://github.com/astral-sh/ty/issues/2100
synced_at: 2026-01-10T01:56:41Z
```

# ty not working with Marimo's VSCode extension

---

_Issue opened by @hierr on 2025-12-19 05:56_

### Summary

Ty doesn't seem to be working with [Marimo's VSCode extension](https://github.com/marimo-team/marimo-lsp).  May be related in some way with [this ruff issue](https://github.com/astral-sh/ruff-vscode/issues/893)

### Version

ty 0.0.4

---

_Renamed from "Ty doesn't work with Marimo's VSCode extension" to "Ty not working with Marimo's VSCode extension" by @hierr on 2025-12-19 05:57_

---

_Comment by @MichaReiser on 2025-12-19 07:23_

Thank you. With not working, I assume you mean ty doesn't provide any LSP features within a notebook? 

Yes, I think this is the expected result of their "mitigation" mentioned here https://github.com/astral-sh/ruff-vscode/issues/893#issuecomment-3573658501

But they also mention

> and it effectively disables Ruff (and other Python extensions for our notebooks). We already have a custom LSP with ty for type-checking (and autocomplete) with our out-of-order cells, and I'm working on a PR to have a managed version of Ruff similarly running inside our extension for our notebooks (https://github.com/marimo-team/marimo-lsp/pull/262).

which would suggest that ty should work? CC: @manzt

---

_Label `notebook` added by @MichaReiser on 2025-12-19 07:23_

---

_Comment by @hierr on 2025-12-19 19:19_

Yes, sorry for the vagueness. Out of the box the LSP features were not being provided when editing the Marimo notebook using the extension. Enabling the config `marimo.disableManagedLanguageFeatures` for suppressing the managed language features makes them show up, but throwing errors, giving the wrong contextual information and "go to definition"turning off the notebook inteface of the extension. Type checking seems to work.

<img width="717" height="358" alt="Image" src="https://github.com/user-attachments/assets/8f7cf0f7-a618-4ccd-aa8a-8ac1d9f08a09" />


<img width="665" height="286" alt="Image" src="https://github.com/user-attachments/assets/09c9f33d-218d-4f42-9608-6d727142f9f6" />


<img width="1597" height="305" alt="Image" src="https://github.com/user-attachments/assets/b6415fdd-2f5f-4e79-93a5-dbb7a4a010c0" />

---

_Renamed from "Ty not working with Marimo's VSCode extension" to "ty not working with Marimo's VSCode extension" by @dhruvmanila on 2025-12-23 11:36_

---
