```yaml
number: 1330
title: "Shutdown request fails with \"invalid type: map, expected unit\" when params is empty object"
type: issue
state: open
author: achyudh
labels:
  - server
assignees: []
created_at: 2025-10-10T02:28:07Z
updated_at: 2025-10-15T15:08:24Z
url: https://github.com/astral-sh/ty/issues/1330
synced_at: 2026-01-12T15:54:25Z
```

# Shutdown request fails with "invalid type: map, expected unit" when params is empty object

---

_@achyudh_

### Summary

The `ty` language server rejects LSP `shutdown` requests when the `params` field contains an empty JSON object `{}`, causing the server to crash instead of shutting down gracefully. Emacs eglot [recently changed](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=66144) to send `{}` instead of `null` to accommodate other language servers (like `ocamllsp`) that have strict validation.

### Error Message

```
[eglot] Server reports (type=1): ty failed to handle a request from the editor. Check the logs for more details.
[jsonrpc] Server exited with status 9
jsonrpc-error: "request id=5 failed:", (jsonrpc-error-code . -32603), 
  (jsonrpc-error-message . "JSON parsing failure: Invalid request Method: shutdown 
  error: invalid type: map, expected unit")
```

### Steps to Reproduce

1. Start `ty` language server via Emacs eglot
2. Send a `shutdown` request with `M-x eglot-shutdown`
3. Server crashes with the above error

### Expected Behavior

According to the [JSON-RPC 2.0 specification, section 4.2](https://www.jsonrpc.org/specification#parameter_structures):

> **If present**, parameters for the rpc call MUST be provided as a Structured value. Either by-position through an Array or by-name through an Object.

An empty object `{}` is a valid structured value and should be accepted. The LSP specification inherits this from JSON-RPC.

### Related Issues
- Emacs bug report: https://debbugs.gnu.org/cgi/bugreport.cgi?bug=66144
- Similar issue was reported for helix: https://github.com/helix-editor/helix/issues/5400

### Version

- `ty` version: 0.0.1-alpha.21
- Editor: Emacs 31.0.50 with eglot
- OS: Ubuntu Linux 6.8.0-83-generic 

---

_Label `server` added by @AlexWaygood on 2025-10-10 06:43_

---

_Comment by @MichaReiser on 2025-10-10 06:56_

This might require changes in the `lsp-server` or `lsp-types` crate. I'm also not sure if we support the array notation for params but I don't think that's important. 

I suspect that rust anlayzer has the same issue

---

_Comment by @achyudh on 2025-10-10 16:54_

Can confirm that rust-analyzer has the same issue:
> jsonrpc-request: jsonrpc-error: "request id=6 failed:", (jsonrpc-error-code . -32602), (jsonrpc-error-message . "Failed to deserialize shutdown: invalid type: map, expected unit; {}"), (jsonrpc-error-data)

Given this is the case, I am assuming this first needs to be fixed upstream.

---

_Renamed from "`shutdown` request fails with "invalid type: map, expected unit" when params is empty object" to "Shutdown request fails with "invalid type: map, expected unit" when params is empty object" by @achyudh on 2025-10-15 15:07_

---

_Comment by @achyudh on 2025-10-15 15:08_

I have opened an issue with rust-analyzer to track this: https://github.com/rust-lang/rust-analyzer/issues/20846

---
