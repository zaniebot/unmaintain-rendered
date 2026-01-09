---
number: 17010
title: "[Panic] Server crashes when setting `exclude` in VS Code settings"
type: issue
state: open
author: flying-sheep
labels:
  - bug
  - configuration
  - server
assignees: []
created_at: 2025-03-27T10:29:59Z
updated_at: 2025-03-27T19:50:28Z
url: https://github.com/astral-sh/ruff/issues/17010
synced_at: 2026-01-07T13:12:16-06:00
---

# [Panic] Server crashes when setting `exclude` in VS Code settings

---

_Issue opened by @flying-sheep on 2025-03-27 10:29_

I tried to exclude this (literal, but weird-looking) path used by our template because Ruff complains about not being able to parse the templated pyproject.toml in there. But the Ruff server doesn’t like that exclude either, apparently.

```jsonc
{
  "ruff.exclude": ["{{cookiecutter.project_name}}"],
}
```

```
thread 'main' panicked at crates/ruff_server/src/session/index/ruff_settings.rs:117:82:
editor configuration should merge successfully with default configuration: error parsing glob '/home/phil/Dev/Python/Single Cell/cookiecutter-scverse/{{cookiecutter.project_name}}': nested alternate groups are not allowed
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[Error - 11:27:07] Client Ruff: connection to server is erroring. Shutting down server.
[Error - 11:27:07] Sending document notification textDocument/didOpen failed
Error: write EPIPE
	at afterWriteDispatched (node:internal/stream_base_commons:161:15)
	at writeGeneric (node:internal/stream_base_commons:152:3)
	at Socket._writeGeneric (node:net:958:11)
	at Socket._write (node:net:970:8)
	at writeOrBuffer (node:internal/streams/writable:572:12)
	at _write (node:internal/streams/writable:501:10)
	at Writable.write (node:internal/streams/writable:510:10)
	at /home/phil/.vscode/extensions/charliermarsh.ruff-2025.22.0-linux-x64/dist/extension.js:1:124667
	at new Promise (<anonymous>)
	at a.write (/home/phil/.vscode/extensions/charliermarsh.ruff-2025.22.0-linux-x64/dist/extension.js:1:124585)
	at y.doWrite (/home/phil/.vscode/extensions/charliermarsh.ruff-2025.22.0-linux-x64/dist/extension.js:1:112201)
	at /home/phil/.vscode/extensions/charliermarsh.ruff-2025.22.0-linux-x64/dist/extension.js:1:112096
[Error - 11:27:07] Connection to server got closed. Server will not be restarted.
[Error - 11:27:07] Stopping server failed
  Message: Cannot call write after a stream was destroyed
  Code: -32099 
[Error - 11:27:07] Stopping server failed
  Message: Cannot call write after a stream was destroyed
  Code: -32099 
```

---

_Comment by @flying-sheep on 2025-03-27 10:31_

Btw, I read the error message and fixed it by using escapes, no worries!

Just filed this issue so you can keep it from crashing!

Then again, it doesn’t work: Ruff still tries to parse the pyproject.toml in there. Any way to keep it from doing that?

---

_Label `bug` added by @dylwil3 on 2025-03-27 10:51_

---

_Label `configuration` added by @dylwil3 on 2025-03-27 10:51_

---

_Comment by @dylwil3 on 2025-03-27 10:52_

Thanks! 

Would you be able to provide some more information about the directory structure here, or, even better, a minimal reproducible example? And does this problem happen only with VSCode or does it also happen when you invoke `ruff` from the command line?

---

_Label `needs-mre` added by @dylwil3 on 2025-03-27 10:53_

---

_Comment by @flying-sheep on 2025-03-27 12:00_

I don’t think the directory structure matters, just having an unescaped `{{` in your `ruff.exclude`, so the original comment should contain your minimum example!

It happens only in the extension, so this should probably be moved to the ruff-vscode repo, sorry about that!

The link in the panic itself directed me here, so maybe you should make it so when the extension’s server panics, people are directed to there?

---

_Comment by @dylwil3 on 2025-03-27 13:01_

> It happens only in the extension, so this should probably be moved to the ruff-vscode repo, sorry about that!

Well it could be an issue with `ruff server` not the VSCode per se, so I think this is the right spot

> I don’t think the directory structure matters, just having an unescaped {{ in your ruff.exclude, so the original comment should contain your minimum example!

Ah I was confused, I thought you also had to have an invalid `pyproject.toml` somewhere, but you're saying just the unescaped braces in the VSCode settings is causing this. Okay, I'll try to reproduce.

---

_Comment by @flying-sheep on 2025-03-27 13:24_

> Ah I was confused, I thought you also had to have an invalid pyproject.toml somewhere,

Sorry for the confusion: the invalid pyproject.toml is irrelevant, it’s just what prompted me to want `{{` in a path, which is kinda uncommon.

If you know how to keep the Ruff extension from trying to parse that file, I’d appreciate it, but it’s not necessary to cause this bug.

---

_Comment by @ntBre on 2025-03-27 17:03_

This sounds related to https://github.com/astral-sh/ruff/pull/16407, which fixed a similar issue in the CLI. This must be a different globbing call in the server.

Fortunately it was pretty easy to fix with [`globset::escape`](https://docs.rs/globset/latest/globset/fn.escape.html), or we could possibly try to reuse the `GlobPath` type I added.

---

_Label `needs-mre` removed by @MichaReiser on 2025-03-27 19:50_

---

_Label `server` added by @MichaReiser on 2025-03-27 19:50_

---
