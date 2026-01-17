```yaml
number: 2545
title: "ERROR An error occurred with request ID 301: request handler panicked at ..."
type: issue
state: open
author: IlyaMaksheev
labels:
  - server
  - fatal
assignees: []
created_at: 2026-01-17T11:38:33Z
updated_at: 2026-01-17T11:40:48Z
url: https://github.com/astral-sh/ty/issues/2545
synced_at: 2026-01-17T12:09:25Z
```

# ERROR An error occurred with request ID 301: request handler panicked at ...

---

_@IlyaMaksheev_

### Summary

Here is code, where I encountered the issue:

```python
    async def _run(self) -> None:
        try:
            ...

        except | # My cursor was here

        except* Exception as exception_group:
            ...

            raise
```

Here is log I got in neovim lsp:

```text
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.954519949  INFO Version: 0.0.12 (4b74e4ded 2026-01-14)\n"
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.971742069  INFO Defaulting to python-platform `linux`\n"
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.974017329  INFO Python version: Python 3.14, platform: linux\n"
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.976542167  WARN Your LSP client doesn't support file watching: You may see stale results when files change outside the editor\n"
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.976555806  WARN Received notification workspace/didChangeConfiguration which does not have a handler.\n"
[ERROR][2026-01-17 16:21:40] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:21:40.983523806  INFO Indexed 136 file(s) in 0.002s\n"
[ERROR][2026-01-17 16:29:02] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:29:02.542414956 ERROR An error occurred with request ID 301: request handler panicked at crates/ty_python_semantic/src/semantic_model.rs:584:1:\nno entry found for keyquery stacktrace:\n\n\nrun with `RUST_BACKTRACE=1` environment variable to display a backtrace\n\n"
[ERROR][2026-01-17 16:29:05] /usr/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ty"	"stderr"	"2026-01-17 16:29:05.959816215 ERROR An error occurred with request ID 319: request handler panicked at crates/ty_python_semantic/src/semantic_model.rs:584:1:\nno entry found for keyquery stacktrace:\n\n\nrun with `RUST_BACKTRACE=1` environment variable to display a backtrace\n\n"
```

Should I give more info?

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Label `server` added by @AlexWaygood on 2026-01-17 11:40_

---

_Label `fatal` added by @AlexWaygood on 2026-01-17 11:40_

---

_Comment by @AlexWaygood on 2026-01-17 11:40_

Thanks very much! This looks similar to https://github.com/astral-sh/ty/issues/2401

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-17 11:40_

---
