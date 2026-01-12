```yaml
number: 2032
title: "Feature: Support for setting virtual environment path in LSP settings"
type: issue
state: open
author: joshzcold
labels:
  - question
  - configuration
  - server
assignees: []
created_at: 2025-12-17T19:55:39Z
updated_at: 2025-12-31T15:43:54Z
url: https://github.com/astral-sh/ty/issues/2032
synced_at: 2026-01-12T15:54:26Z
```

# Feature: Support for setting virtual environment path in LSP settings

---

_@joshzcold_

### Question

Support for setting the virtual env path via lsp settings aswell as `VIRTUAL_ENV`

`basepyright` supports this in the form of 

> python.venvPath [path]: Path to folder with subdirectories that contain virtual environments. The python.pythonPath setting is recommended over this mechanism for most users. For more details, refer to the [import resolution](https://docs.basedpyright.com/v1.23.1/usage/import-resolution/#configuring-your-python-environment) documentation.

https://docs.basedpyright.com/v1.23.1/configuration/language-server-settings/

What this allows is editors to have the ability to configure the path to virtual environment dynamically instead of having to set it in the OS environment and restarting the lsp server.

See my plugins interaction with `basedpyright` for details.
https://github.com/joshzcold/python.nvim/blob/main/lua/python/lsp/init.lua#L23

### Version

ty 0.0.2

---

_Label `question` added by @joshzcold on 2025-12-17 19:55_

---

_Label `configuration` added by @carljm on 2025-12-17 21:31_

---

_Label `server` added by @carljm on 2025-12-17 21:31_

---

_Comment by @Gankra on 2025-12-17 22:48_

In vscode we explicitly depend on the generic python extension to provide this functionality. Although you might be able to use [the `[tool.ty.environment]` setting](https://docs.astral.sh/ty/reference/configuration/#extra-paths)?

See also:

* https://github.com/astral-sh/ty/issues/1185
* https://github.com/astral-sh/ty-vscode/issues/5
* https://github.com/astral-sh/ty/issues/536

---

_Comment by @joshzcold on 2025-12-17 23:12_

> In vscode we explicitly depend on the generic python extension to provide this functionality. Although you might be able to use [the [tool.ty.environment] setting](https://docs.astral.sh/ty/reference/configuration/#extra-paths)?

My use case is somewhat unqiue and requires the ability to change the venv path while the lsp server is running.

I maintain a plugin for neovim dev in python.
https://github.com/joshzcold/python.nvim

This plugin supports a number of lsp servers when the user wants to change their venv.

pyright and basedpyright allows me to send a new venv location to the server while the editor is still running. pyright respondes to this by re-reading the workspace with the new venv in the lsp server.
https://github.com/joshzcold/python.nvim/blob/main/lua/python/lsp/init.lua#L29

Most users will probably get by with `uv` and `.venv`, but for users outside of the `uv` ecosystem they might have their venv in the `~/.virtual_env` directory.

So this might not be "nessesary", but might be a nice addition to the lsp server.

I probably can get by with `VIRTUAL_ENV` and some fix to #2031  to have the LSP server restart gracefully.


---

_Comment by @MichaReiser on 2025-12-18 07:26_

Have you tried what I described here https://github.com/astral-sh/ty/issues/2010#issuecomment-3665555239? 

This is what both Zed and VS Code use to change the python environment at runtime

---

_Comment by @joshzcold on 2025-12-18 18:10_

@MichaReiser Does vscode and zed restart the lsp to pick up the new setting from the start? I don't think ty server has a mechanism to update the configuration via the client?

Theres probably a way for me to stop and launch the `ty` server with new settings in neovim, but I think that would be unique logic from my plugin and not something as standard as `workspace/didChangeConfiguration` ?

```
WARN Received notification workspace/didChangeConfiguration which does not have a handler.\n"
```
https://github.com/astral-sh/ruff/blob/c31516473296a8b745af946b827d3342ef81060f/crates/ty_server/src/server/api.rs#L111



---

_Comment by @MichaReiser on 2025-12-18 18:14_

Yes, Zed and VS Code restart ty on any configuration change because ty doesn't yet support `didChangeConfiguration`. I don't think it would be very hard to add support for `didChangeConfiguration`; it's just not a high-priority feature. 

---

_Comment by @purepani on 2025-12-19 00:06_

Being able to set this would also help with opening scripts with inline metadata!

---

_Comment by @joshzcold on 2025-12-19 15:45_

Could break this down into 2 more focused feature requests

- support for settings the environment in lsp settings. Very close to https://github.com/astral-sh/ty/issues/2010#issuecomment-3665555239 but in an easy to use fashion (something that could benefit the hack in the vscode extension)
- support for `workspace/didChangeConfiguration` which would benefit the vscode extension and others by not needed to restart the server on updates.

---

_Comment by @MichaReiser on 2025-12-19 16:18_

The first point is covered by https://github.com/astral-sh/ruff/pull/22053

> but in an easy-to-use fashion (something that could benefit the hack in the VS Code extension)

I don't think so. We still need the "hack" because an explicitly configured virtual environment using ty's configuration takes precedence over the interpreter selected in VS Code

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:43_

---
