```yaml
number: 2353
title: Stale diagnostics when content is updated outside the editor
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-06T05:37:17Z
updated_at: 2026-01-06T10:52:54Z
url: https://github.com/astral-sh/ty/issues/2353
synced_at: 2026-01-12T15:54:26Z
```

# Stale diagnostics when content is updated outside the editor

---

_@dhruvmanila_

### Summary

As seen in the following video, when I add new content to the file using `echo`, the diagnostics isn't updated:

https://github.com/user-attachments/assets/a70fc8b2-5753-4cb1-85da-5b4ad81038a5

Here's the code:

```py
import os

import something
```

And, then use the following command to append the file with a new unknown import:

```bash
echo "import other\n" >> src/ty_server/bug.py
```

Here's what Neovim is sending when the above command is executed which is a `didClose` notification followed by a `didOpen` notification with the updated file content:

```
2026-01-06 11:02:09 [DEBUG] rpc.send {
  jsonrpc = "2.0",
  method = "textDocument/didClose",
  params = {
    textDocument = {
      uri = "file:///Users/dhruv/playground/ty-server/src/ty_server/bug.py"
    }
  }
}
2026-01-06 11:02:09 [DEBUG] rpc.send {
  jsonrpc = "2.0",
  method = "textDocument/didOpen",
  params = {
    textDocument = {
      languageId = "python",
      text = "import os\n\nimport something\nimport other\n\n",
      uri = "file:///Users/dhruv/playground/ty-server/src/ty_server/bug.py",
      version = 0
    }
  }
}
```

And, here's the logs which says that ty thinks the diagnostics are unchanged:

```
2026-01-06 11:02:09 [DEBUG] rpc.send {
  id = 6,
  jsonrpc = "2.0",
  method = "textDocument/diagnostic",
  params = {
    previousResultId = "919997a55b1181a2",
    textDocument = {
      uri = "file:///Users/dhruv/playground/ty-server/src/ty_server/bug.py"
    }
  }
}
2026-01-06 11:02:09 [DEBUG] rpc.receive {
  id = 6,
  jsonrpc = "2.0",
  result = {
    kind = "unchanged",
    resultId = "919997a55b1181a2"
  }
}
```

These are the ty server's debug logs during the `didClose` and `didOpen` events:
```
2026-01-06 11:02:09.644047000 DEBUG notification{method="textDocument/didClose"}: Closing file `/Users/dhruv/playground/ty-server/src/ty_server/bug.py`
2026-01-06 11:02:09.644209000 DEBUG notification{method="textDocument/didClose"}: Take open project files
2026-01-06 11:02:09.644357000 DEBUG notification{method="textDocument/didClose"}:set_open_files{open_files={}}: Set open project files (count: 0)
2026-01-06 11:02:09.644687000 DEBUG notification{method="textDocument/didOpen"}: Opening file `/Users/dhruv/playground/ty-server/src/ty_server/bug.py`
2026-01-06 11:02:09.644716000 DEBUG notification{method="textDocument/didOpen"}: Take open project files
2026-01-06 11:02:09.644771000 DEBUG notification{method="textDocument/didOpen"}:set_open_files{open_files={file(Id(c09))}}: Set open project files (count: 1)
```

### Version

ty ruff/0.14.10+203 (e63cf978a 2026-01-05)

---

_Label `bug` added by @dhruvmanila on 2026-01-06 05:37_

---

_Label `server` added by @dhruvmanila on 2026-01-06 05:37_

---

_Comment by @dhruvmanila on 2026-01-06 05:37_

#2346 might be related

---

_Added to milestone `Stable` by @dhruvmanila on 2026-01-06 05:38_

---

_Comment by @MichaReiser on 2026-01-06 09:34_

> [#2346](https://github.com/astral-sh/ty/issues/2346) might be related

Yes, I think this is the same

---

_Closed by @MichaReiser on 2026-01-06 10:52_

---
