```yaml
number: 22536
title: "`ruff server` ignores config from nested directory if files are opened directly"
type: issue
state: closed
author: ZedThree
labels: []
assignees: []
created_at: 2026-01-12T17:11:02Z
updated_at: 2026-01-12T18:06:07Z
url: https://github.com/astral-sh/ruff/issues/22536
synced_at: 2026-01-12T18:23:23Z
```

# `ruff server` ignores config from nested directory if files are opened directly

---

_@ZedThree_

### Summary

Here's a simple example:

```
└── dir
    ├── ruff.toml
    └── test.py
```

`dir/test.py`:
```py
import os
````

`dir/ruff.toml`:
```toml
[lint]
select = ["FURB"]
preview = true
```

`ruff check` gives the expected behaviour: no warnings. Opening the top-level dir or `dir_1` in VS Code also gives the expected behaviour.

However, opening the file directly does give a warning (`F401`). This also happens with helix, neovim, and emacs (which don't all necessarily have opening a folder as a workspace as a concept).

Creating `pyproject.toml` at the top level resolves the issue.

This might be a duplicate of #20847 and #17944, which are similar issues where the server is not resolving config correctly.

### Version

ruff 0.14.11

---

_Comment by @MichaReiser on 2026-01-12 17:55_

Thanks for the great write up and also mentioning the related issues. The root cause for this is the same as https://github.com/astral-sh/ruff/issues/17944, Ruff doesn't even try to find settings for files outside the workspace root. 

---

_Closed by @MichaReiser on 2026-01-12 17:55_

---

_Comment by @ZedThree on 2026-01-12 18:01_

Well, not quite -- in this case, it's _below_ the workspace root:

```
DEBUG Indexing settings for workspace: /tmp/mvce
 INFO Registering workspace: /tmp/mvce
DEBUG Received message: Notification(Notification { method: "textDocument/didOpen", params: Object {"textDocument": Object {"languageId": String("python"), "text": String("import os\n"), "uri": String("file:///tmp/mvce/dir/test.py"), "version": Number(0)}} })
```

---

_Comment by @MichaReiser on 2026-01-12 18:06_

> Well, not quite -- in this case, it's below the workspace root:

I understood that it's working if you open the folder (in which case the workspace root is `/tmp/mvce`) but not when opening the file (in which case I believe VS Code uses `~/`)

---
