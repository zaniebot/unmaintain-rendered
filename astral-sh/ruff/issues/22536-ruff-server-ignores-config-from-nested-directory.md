```yaml
number: 22536
title: "`ruff server` ignores config from nested directory if files are opened directly"
type: issue
state: closed
author: ZedThree
labels: []
assignees: []
created_at: 2026-01-12T17:11:02Z
updated_at: 2026-01-12T18:29:08Z
url: https://github.com/astral-sh/ruff/issues/22536
synced_at: 2026-01-12T19:26:13Z
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

_Comment by @ZedThree on 2026-01-12 18:21_

Ah, indeed, that is true with VS code, but with other editors like neovim and helix, the workspace root is the cwd, so `/tmp/mvce` in this case. Here's the logs from neovim from first `nvim dir/test.py`, then from `cd dir; nvim test.py`

```
 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce
 INFO No workspace options found for file:///tmp/mvce, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce
 INFO Registering workspace: /tmp/mvce
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir/test.py
DEBUG request{id=3 method="shutdown"}: enter
DEBUG request{id=3 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down


 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce/dir
 INFO No workspace options found for file:///tmp/mvce/dir, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce/dir
DEBUG No `target-version` found in `ruff.toml` log.target="ruff_workspace::pyproject" log.module_path="ruff_workspace::pyproject" log.file="crates/ruff_workspace/src/pyproject.rs" log.line=207
DEBUG No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified log.target="ruff_workspace::pyproject" log.module_path="ruff_workspace::pyproject" log.file="crates/ruff_workspace/src/pyproject.rs" log.line=218
DEBUG Loaded settings from: `/tmp/mvce/dir/ruff.toml`
 INFO Registering workspace: /tmp/mvce/dir
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir/test.py
DEBUG request{id=3 method="shutdown"}: enter
DEBUG request{id=3 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down
```

---

_Comment by @MichaReiser on 2026-01-12 18:29_

@dhruvmanila will know more here but the editors still run in single-file mode

https://github.com/astral-sh/ruff/blob/4c4ddc8c29e649dad381b21c90593226687825e1/crates/ruff_server/src/session/index/ruff_settings.rs#L237-L254

which was added here https://github.com/astral-sh/ruff/pull/13770

That's why I still think it's the same issue. Ruff uses the default settings for files outside the workspace root (which also applies if the editor sends no workspace root)

---
