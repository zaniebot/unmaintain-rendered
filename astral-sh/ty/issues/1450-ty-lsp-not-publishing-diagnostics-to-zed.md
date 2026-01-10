```yaml
number: 1450
title: ty lsp not publishing diagnostics to Zed
type: issue
state: closed
author: insane-dreamer
labels:
  - question
  - server
assignees: []
created_at: 2025-10-28T23:49:07Z
updated_at: 2025-11-06T00:45:35Z
url: https://github.com/astral-sh/ty/issues/1450
synced_at: 2026-01-10T02:06:25Z
```

# ty lsp not publishing diagnostics to Zed

---

_Issue opened by @insane-dreamer on 2025-10-28 23:49_

### Summary

I'll preface this by saying I don't know whether this is an issue with the `ty` server or `zed`. But despite ty support being implemented in zed, I'm not getting any diagnostics. 

I've verified from the logs:
  - ty LSP starts and tracks files in Zed 
  - ty registers diagnostics capabilities 
  - ty isn't publishing diagnostics (not showing up in ty logs, also not in Zed)
  - no error messages in either ty or zed logs 

(ty check works from CLI and flags errors as expected)
 


### Version

0.0.1-alpha.24 on Ubuntu 24.04

---

_Label `server` added by @AlexWaygood on 2025-10-28 23:58_

---

_Label `question` added by @MichaReiser on 2025-10-29 00:59_

---

_Comment by @MichaReiser on 2025-10-29 01:01_

Can you tell me a bit more about your setup?

* What Zed version are you using?
* What's your Zed configuration?
* How do you tell that ty tracks the files in Zed?
* Did you enable workspace diagnostics? If not, did you open a file with a diagnostic (in which case Zed should send a pullDiagnostic request)
* If you can, can you share the LSP server logs?

---

_Comment by @insane-dreamer on 2025-10-29 04:18_

I just saw another `zed` user opened an issue about not getting diagnostics from `ty` or `ruff`. My `zed` install (v 0.209.7) is showing diagnostics from `basedpyright` but not `ruff` or `ty`. Also tested  `pyrefly` -- no diagnostics either, so that indicates the issue is probably on the zed side. I'll see if that gets resolved and if not report back. 


---

_Comment by @MichaReiser on 2025-10-29 13:05_

Thanks. I can confirm that ty stopped working for me too (in Zed). 

I scanned the RPC messages and Zed doesn't request any diagnostics (and ty doesn't send any diagnostics using the push model because Zed tells us that it supports pull diagnostics). 

---

_Comment by @MichaReiser on 2025-10-29 13:08_

Upstream Zed issue https://github.com/zed-industries/zed/pull/41386

---

_Comment by @MichaReiser on 2025-11-04 18:57_

Second upstream issue https://github.com/zed-industries/zed/pull/41929

---

_Comment by @andythomas on 2025-11-04 22:12_

After some boring hours of debugging (or rather random trying and hoping for the best ;), I could make my Zed work again. I found https://github.com/zed-industries/zed/issues/39499 and https://github.com/astral-sh/ty/issues/1299 and https://github.com/zed-industries/zed/issues/39144, which made me change from

```json
"lsp": {
    "ty": {
      "binary": {
        "path": ".venv/bin/ty",
        "arguments": ["server"]
      }
    },
```

to

```json
"lsp": {
    "ty": {
      "binary": {
        "path": ".venv/bin/ty",
        "arguments": ["server"]
      },
      "settings": {
        "diagnosticMode": "workspace"
      }
    }, 
```

And lo and behold it works again. Before I tried Zed 0.210.4 and Zed Preview 0.211.3 but to no avail. Now, both work.

So, overall: explicit `"diagnosticMode": "workspace"` works as intended, but without it, not even the open files are scanned. 

Hopefully, somebody might find this earlier :)

---

_Comment by @insane-dreamer on 2025-11-05 23:40_

Confirming that with latest Zed release (211.4) `ty` diagnostics are now showing in Zed when using the above settings. I tried both `workspace` and `openFilesOnly` diagnosticMode, and both worked as expected. Omitting the `diagnosticMode` altogether defaults to open files only (and works). 
This is on Ubuntu 24.04. 



---

_Comment by @MichaReiser on 2025-11-06 00:45_

Thanks for the update. I tested it myself with Zed and Zed Preview and the issue is fixed for both.

---

_Closed by @MichaReiser on 2025-11-06 00:45_

---
