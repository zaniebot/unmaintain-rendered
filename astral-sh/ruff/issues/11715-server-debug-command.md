```yaml
number: 11715
title: "Server: Debug command"
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2024-06-03T06:44:35Z
updated_at: 2024-06-11T18:42:47Z
url: https://github.com/astral-sh/ruff/issues/11715
synced_at: 2026-01-12T15:54:51Z
```

# Server: Debug command

---

_@MichaReiser_

Maybe ruff extension could provide a command to show current server info? This will be helpful to debugging.

_Originally posted by @yanyongyu in https://github.com/astral-sh/ruff/issues/11674#issuecomment-2144207242_

Goal: Add a LSP command to collect debugging relevant information (platform, architecture, server, number of open files, number of open workspaces, the values of the most important settings, ...)
            

---

_Label `server` added by @MichaReiser on 2024-06-03 06:44_

---

_Renamed from "Server: Version command" to "Server: Debug command" by @MichaReiser on 2024-06-03 06:44_

---

_Comment by @dhruvmanila on 2024-06-03 09:39_

Related: https://rust-analyzer.github.io/manual.html#status

---

_Comment by @dhruvmanila on 2024-06-03 10:40_

I think a simple implementation to show Ruff's executable path and version number would be useful to begin with.

---

_Assigned to @snowsignal by @snowsignal on 2024-06-03 20:38_

---

_Comment by @rainzee on 2024-06-04 02:35_

> I think a simple implementation to show Ruff's executable path and version number would be useful to begin with.

In vsc's extension, the binary that use for ruff finds from different places until fallback(use bundled), but the information is missing in the output message, is it possible to add the path and version number of the binary that is currently using ruff in the output message?

---

_Comment by @dhruvmanila on 2024-06-04 05:59_

> In vsc's extension, the binary that use for ruff finds from different places until fallback(use bundled), but the information is missing in the output message, is it possible to add the path and version number of the binary that is currently using ruff in the output message?

Which output message are you referring to? The goal for this issue is to implement that "output message" functionality. But to answer your question, yes we will be including that information in the message.

---

_Closed by @snowsignal on 2024-06-11 18:42_

---
