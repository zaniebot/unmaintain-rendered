```yaml
number: 22609
title: "client:supports_method(callHierarchy/outgoingCalls) is incorrect"
type: issue
state: closed
author: bvdmitri
labels:
  - server
assignees: []
created_at: 2026-01-15T19:01:03Z
updated_at: 2026-01-16T08:23:00Z
url: https://github.com/astral-sh/ruff/issues/22609
synced_at: 2026-01-16T08:55:06Z
```

# client:supports_method(callHierarchy/outgoingCalls) is incorrect

---

_@bvdmitri_

### Summary

I'm not have enough knowledge here, so I simply link this comment that suggests something is wrong with ruff LSP implementation 

https://github.com/ibhagwan/fzf-lua/issues/2247#issuecomment-3741284645

> just tested this is an issue with ruff, it returns client:supports_method(textDocument/prepareCallHierarchy) = false (correct) but client:supports_method(callHierarchy/outgoingCalls)=true (incorrect), when fzf-lua sends the incoming/outgoing call query it fails with ruff and fzf-lua reports about it.

> Open an issue upstream with ruff if you want this to be fixed.

TLDR: 

`:FzfLua lsp_incoming_calls` command within NeoVim fails if used in combination with ruff and basedpyright LSP 

### Version

  - Version: 0.14.11

---

_Label `server` added by @ntBre on 2026-01-15 19:53_

---

_Comment by @MichaReiser on 2026-01-16 08:23_

I commented upstream because Ruff never registers support for any call hierarchy capability. Let's track this upstream. We can re-open if necessary

---

_Closed by @MichaReiser on 2026-01-16 08:23_

---
