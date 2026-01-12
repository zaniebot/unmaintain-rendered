```yaml
number: 953
title: "Add support for `workspace/didChangeConfiguration` notification"
type: issue
state: open
author: dhruvmanila
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-08-07T14:00:11Z
updated_at: 2025-11-18T16:10:35Z
url: https://github.com/astral-sh/ty/issues/953
synced_at: 2026-01-12T15:54:24Z
```

# Add support for `workspace/didChangeConfiguration` notification

---

_@dhruvmanila_

This is a follow-up task from https://github.com/astral-sh/ty/issues/82 which is to add support for handling `workspace/didChangeConfiguration` notification.

When the server receives the `workspace/didChangeConfiguration` notification, the server should refresh the settings from all the managed workspaces, ignoring any values in the notification payload.

This would allow users to dynamically update the client settings without the need to restart the server.

Once this is implemented, we should also remove [these settings](https://github.com/astral-sh/ty-vscode/blob/38d25d5d2c561ecd910858ca23081cd97d503e6c/src/common/settings.ts#L118-L122) which is used to restart the server when the value change in ty VS Code extension.

---

_Label `server` added by @dhruvmanila on 2025-08-07 14:00_

---

_Added to milestone `GA` by @dhruvmanila on 2025-08-07 14:00_

---

_Label `help wanted` added by @MichaReiser on 2025-08-07 14:06_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-11-14 09:08_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 09:08_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
