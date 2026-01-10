```yaml
number: 2010
title: How do I pass my Python interpreter to Ty in Neovim?
type: issue
state: closed
author: IceS2
labels:
  - question
  - configuration
  - editor
assignees: []
created_at: 2025-12-17T13:50:04Z
updated_at: 2025-12-18T07:26:54Z
url: https://github.com/astral-sh/ty/issues/2010
synced_at: 2026-01-10T01:53:59Z
```

# How do I pass my Python interpreter to Ty in Neovim?

---

_Issue opened by @IceS2 on 2025-12-17 13:50_

### Question

I've been trying a couple different things with version 0.0.2 and I keep receiving an error similar to the following one:

```
LSP[ty][Warning] Received unknown options for workspace 
`file:....`: {
  "environment": {
    "python": "/env/bin/python"
  }
} 
```

This is what I'm doing:

```
   settings = {
     ty = {
       environment = {
         python = get_python_path(),
       },
   },
```

### Version

_No response_

---

_Label `question` added by @IceS2 on 2025-12-17 13:50_

---

_Comment by @sharkdp on 2025-12-17 13:58_

Thank you for reporting this.

`environment.python` is not part of the LSP server options, but rather part of ty options (`ty.toml` or `pyproject.toml`). See https://docs.astral.sh/ty/configuration/#configuration-files for more details.

---

_Label `configuration` added by @AlexWaygood on 2025-12-17 13:58_

---

_Label `editor` added by @AlexWaygood on 2025-12-17 13:58_

---

_Comment by @MichaReiser on 2025-12-17 13:58_

We do plan on exposing some configuration in the future, but haven't gotten around to doing it.

---

_Comment by @IceS2 on 2025-12-17 14:05_

I see. Thanks for the quick answer folks!
Is there a way to update the interpreter that the lsp is using? I currently use ruff and pyright with neovim and using whichpy to select a specific virtualenv and restart the lsp with the correct one.

Thanks a lot!

---

_Comment by @MichaReiser on 2025-12-17 14:14_

Not sure, I don't know neovim well myself. We do something "hacky" in VS Code where we pass the selected python interpreter as `ty.pythonExtension.activeEnvironment`, like this


```json
        "pythonExtension": {
            "activeEnvironment": {
                "version": {
                    "major": 3,
                    "minor": 8,
                    "patch": 20,
                    "sysVersion": "3.8.20.final.0"
                },
                "environment": {
                    "folderUri": "file:///Users/micha/astral/discord.py/.venv/bin/python",
                    "uri": "discord.py",
                    "type": "VirtualEnvironment"
                },
                "executable": {
                    "uri": "file:///Users/micha/astral/discord.py/.venv/bin/python",
                    "sysPrefix": "/Users/micha/astral/discord.py/.venv"
                }
            }
        }
```

I think you should be able to do the same from within neovim. 


The alternative is to set `environment.python` in a `pyproject.toml` or `ty.toml`

---

_Comment by @joshzcold on 2025-12-17 23:29_

I created a duplicate issue here: #2032 

---

_Closed by @MichaReiser on 2025-12-18 07:26_

---
