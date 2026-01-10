```yaml
number: 1328
title: Throws error notification on Claude Code edits
type: issue
state: closed
author: AndrewNolte
labels:
  - server
assignees: []
created_at: 2025-10-09T14:33:07Z
updated_at: 2025-12-18T18:01:04Z
url: https://github.com/astral-sh/ty/issues/1328
synced_at: 2026-01-10T01:53:59Z
```

# Throws error notification on Claude Code edits

---

_Issue opened by @AndrewNolte on 2025-10-09 14:33_

### Summary

Logs:
```
ERROR Encountered error when routing notification: JSON parsing failure:
Invalid notification
Method: workspace/didChangeWatchedFiles
 error: relative URL without a base: "_claude_fs_right:<some_abs_path_file>"
ERROR Encountered error when routing notification: JSON parsing failure:
Invalid notification
Method: workspace/didChangeWatchedFiles
 error: relative URL without a base: "_claude_fs_right:<some_abs_path_file>"
```
<some_abs_path_file> was obfuscated.

### Version

2025.43

---

_Label `server` added by @AlexWaygood on 2025-10-09 14:34_

---

_Comment by @sharkdp on 2025-10-09 14:50_

Potentially related to this (https://github.com/anthropics/claude-code/issues/3381)?

>  Claude Code's internal path encoding/resolution creates invalid filesystem URIs on Windows. The **_claude_fs_right**: prefix and URL-encoded path (c%3A%5C) suggest incorrect internal path handling.

Are you on Windows?

---

_Comment by @AndrewNolte on 2025-10-21 20:19_

No, on mac with remote linux

---

_Comment by @AndrewNolte on 2025-10-21 20:21_

Nothing crashes for me, it ty just shows the error notification

---

_Comment by @sharkdp on 2025-10-22 09:33_

I think we need some more information about your specific setup to see if there is anything that can be done about it. At the very least, can you show us that redacted absolute file path (not the actual path, but its structure)?

---

_Label `needs-info` added by @MichaReiser on 2025-11-10 09:55_

---

_Comment by @sharkdp on 2025-12-01 10:04_

I actually ran into this too when Claude was updating a `.txt` file (?!)

<img width="485" height="142" alt="Image" src="https://github.com/user-attachments/assets/d58b4747-12e9-4d05-bb4c-3d145fbb1039" />

```
2025-12-01 10:56:36.521213415 ERROR Encountered error when routing notification: JSON parsing failure:
Invalid notification
Method: workspace/didChangeWatchedFiles
 error: relative URL without a base: "_claude_fs_right:/home/shark/ruff/crates/ty_python_semantic/resources/primer/good.txt"
```

---

_Comment by @sharkdp on 2025-12-01 10:06_

To reproduce, open the ruff repo in VS Code, enable the ty extension, and run something like
```
claude "Add a new line 'foo' at the end of @crates/ty_python_semantic/resources/primer/good.txt"
```

It's apparently this diff view that is causing the problem:

<img width="1084" height="515" alt="Image" src="https://github.com/user-attachments/assets/38bab15b-7725-4785-9552-6ea8193dc53b" />

---

_Comment by @MichaReiser on 2025-12-01 10:14_

I was a bit confused where we're raising this error, but I suspect it's because we use the `Url` for `FileEvent::uri` and deserialization will fail if the `uri` isn't a valid URI. 

However, this is a client issue because the LSP specification also notes:

> Many of the interfaces contain fields that correspond to the URI of a document. For clarity, the type of such a field is declared as a [DocumentUri](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#documentUri). Over the wire, it will still be transferred as a string, but this guarantees that the contents of that string can be parsed as a valid URI.


and `_claude_fs_right:/home/shark/ruff/crates/ty_python_semantic/resources/primer/good.txt` is not a valid URI

For what it's worth, prominently warning users about a notification deserialization error seems unfortunate. I think it should be enough to log a warning, without showing a popup.

---

_Label `needs-info` removed by @MichaReiser on 2025-12-01 10:14_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-01 10:14_

---

_Closed by @MichaReiser on 2025-12-18 18:01_

---
