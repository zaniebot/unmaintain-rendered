---
number: 10780
title: "Ruff-specific source actions are not working with `ruff server`"
type: issue
state: closed
author: snowsignal
labels:
  - bug
  - server
assignees: []
created_at: 2024-04-04T22:34:35Z
updated_at: 2024-04-16T18:21:10Z
url: https://github.com/astral-sh/ruff/issues/10780
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff-specific source actions are not working with `ruff server`

---

_Issue opened by @snowsignal on 2024-04-04 22:34_

VS Code configuration like the following will work as expected:
```json
{
    "[python]": {
      "editor.codeActionsOnSave": {
        "source.organizeImports": "explicit",
      },
    }
}
```

but this configuration:

```json
{
    "[python]": {
      "editor.codeActionsOnSave": {
        "source.organizeImports.ruff": "explicit",
      },
    }
}
```
does not work. 

The problem here is that when we create the source code action, the kind is always set to `source.{fixAll | organizeImports}`. Since the client is expected a code action of kind `source.{fixAll | organizeImports}.ruff`, the code action we return will not get resolved.

---

_Label `server` added by @snowsignal on 2024-04-04 22:34_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-04 22:34_

---

_Label `bug` added by @snowsignal on 2024-04-04 22:40_

---

_Referenced in [astral-sh/ruff#10916](../../astral-sh/ruff/pulls/10916.md) on 2024-04-13 06:27_

---

_Closed by @snowsignal on 2024-04-16 18:21_

---
