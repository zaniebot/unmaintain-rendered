---
number: 11510
title: "`ruff server`: Unable to edit new, untitled files in VS Code"
type: issue
state: closed
author: T-256
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-23T10:51:53Z
updated_at: 2024-05-31T06:34:11Z
url: https://github.com/astral-sh/ruff/issues/11510
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server`: Unable to edit new, untitled files in VS Code

---

_Issue opened by @T-256 on 2024-05-23 10:51_

VSCode Repro: `Ctrl+Win+Alt+N` -> select `Python File`

Output:

> ```
> 2024-05-23 13:52:40.699 [info] Name: Ruff
> 2024-05-23 13:52:40.700 [info] Module: ruff
> 2024-05-23 13:52:40.700 [info] Python extension loading
> 2024-05-23 13:52:40.700 [info] Waiting for interpreter from python extension.
> 2024-05-23 13:52:40.700 [info] Python extension loaded
> 2024-05-23 13:52:40.700 [info] Server run command: c:\Users\User\Desktop\test\.venv\Scripts\python.exe c:\Users\User\.vscode\extensions\charliermarsh.ruff-2024.22.0-win32-x64\bundled\tool\ruff_server.py server --preview
> 2024-05-23 13:52:40.700 [info] Server: Start requested.
> 2024-05-23 13:52:41.180 [info] [Trace - 1:52:41 PM] Received request 'client/registerCapability - (1)'.
> 2024-05-23 13:52:41.180 [info] [Trace - 1:52:41 PM] Sending response 'client/registerCapability - (1)'. Processing request took 0ms
> 2024-05-23 13:52:41.180 [info]    0.139395s INFO ruff_server::server Configuration file watcher successfully registered
> 
> 2024-05-23 13:52:41.736 [info]    0.695387s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
> 
> 2024-05-23 13:52:41.770 [info]    0.729158s ERROR ruff_server::server::api An error occurred while running notebookDocument/didOpen: expected notebook URI untitled:Untitled-1.ipynb?jupyter-notebook to be a valid file path
> 
> 2024-05-23 13:52:41.802 [info]    0.757168s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
> 
> 2024-05-23 13:52:43.029 [info]    1.956011s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
> 
> 2024-05-23 13:52:43.361 [info]    2.319372s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
> 
> 2024-05-23 13:52:43.698 [info]    2.657468s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
> ```



---

_Label `bug` added by @dhruvmanila on 2024-05-23 12:49_

---

_Label `server` added by @dhruvmanila on 2024-05-23 12:49_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-23 15:07_

---

_Renamed from "`ruff server`: Error on Untitled document" to "`ruff server`: Unable to edit new, untitled files in VS Code" by @snowsignal on 2024-05-23 15:10_

---

_Comment by @snowsignal on 2024-05-23 15:11_

Thank you for opening this issue! I've made a small change to the issue title if that's okay with you 🙂 

---

_Comment by @T-256 on 2024-05-24 22:59_

> Thank you for opening this issue! I've made a small change to the issue title if that's okay with you 🙂

Not only on editing Untitled-files but also, on opening them:
```
2024-05-23 13:55:32.141 [info]  171.100002s ERROR ruff_server::server::api An error occurred while running textDocument/didOpen: expected document URI untitled:Untitled-2 to be a valid file path
```

From what I understand, We could avoid using `to_file_path` method of `lsp_types::Url` to solve the problem for non-existent files on disk, so then no need `PathBuf`.
https://github.com/astral-sh/ruff/blob/33fd50027cb24e407746da339bdf2461df194d96/crates/ruff_server/src/edit.rs#L36-L41

Unified `lsp_types::Url` expected at there, then all other urls with non-`file:` scheme would be handled.

---

_Comment by @snowsignal on 2024-05-25 00:08_

Right, at the moment the code assumes that text document URIs point to valid file paths. We'll have to rework how documents are indexed to fix this.

---

_Assigned to @MichaReiser by @MichaReiser on 2024-05-27 16:38_

---

_Referenced in [astral-sh/ruff#11588](../../astral-sh/ruff/pulls/11588.md) on 2024-05-29 21:27_

---

_Closed by @MichaReiser on 2024-05-31 06:34_

---
