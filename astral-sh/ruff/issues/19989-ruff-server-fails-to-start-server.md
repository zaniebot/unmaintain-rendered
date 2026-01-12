```yaml
number: 19989
title: "`ruff server` fails to start server"
type: issue
state: closed
author: Peiffap
labels:
  - question
assignees: []
created_at: 2025-08-19T15:16:38Z
updated_at: 2025-08-21T21:24:50Z
url: https://github.com/astral-sh/ruff/issues/19989
synced_at: 2026-01-12T15:54:57Z
```

# `ruff server` fails to start server

---

_@Peiffap_

### Summary

Running `ruff server` then pressing "Enter" gives me the following:
```zsh
~
❯ ruff server

ruff failed
  Cause: Failed to start server
  Cause: disconnected channel
```
I'm not sure what the expected response is, but I came across this when trying to debug why the `ruff` LSP doesn't work in the Zed editor for me, i.e. the `ruff` server always crashes, with the following log:
```
Language server ruff:

failed to spawn command. path: "/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/Users/gillespeiffer/.local/bin/ruff", working directory: "/Users/gillespeiffer/Documents/foss/networkx", args: ["server"]
-- stderr--
```

Other possibly useful info:
```zsh
~
❯ uv tool install ruff@latest
Resolved 1 package in 73ms
Audited 1 package in 0.43ms
Installed 1 executable: ruff
~
❯ which ruff
/Users/gillespeiffer/.local/bin/ruff

~
❯ ruff version
ruff 0.12.9 (ef422460d 2025-08-14)
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Comment by @ntBre on 2025-08-19 15:30_

I can reproduce this (pressing Enter causing the server to fail), but I think the server is expecting some kind of startup message from the LSP client, not an empty line. That seems to be the case with 0.12.0 too, which was just the last version in my `uvx` shell history. So I don't think that's a good proxy for the Zed issue.

Does Zed let you set any additional flags or environment variables? 0.12.9 seems to be working fine for me in VS Code, but maybe some of the [troubleshooting](https://github.com/astral-sh/ruff-vscode?tab=readme-ov-file#troubleshooting) information from `ruff-vscode` would help here too, if you can set similar options in Zed.

---

_Label `question` added by @ntBre on 2025-08-19 15:34_

---

_Comment by @Peiffap on 2025-08-19 15:34_

While I investigate that troubleshooting page, do you have something I could enter _instead_ of an empty line that you would expect not to fail?

---

_Comment by @ntBre on 2025-08-19 15:47_

No, I was experimenting with that myself, but I'm not having any luck coming up with valid input. I think if it starts up and waits for input (i.e. before you press enter), that's already a good sign. The easiest way to debug from there is to enable logging within the editor, as far as I know.

I also tested this in Zed in the meantime, and 0.12.9 woks fine for me, starting up correctly and showing diagnostics.

Does  invoking the ruff binary from the Zed log work for you? It looks different from the output of `which ruff` in your output above:

```
/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/Users/gillespeiffer/.local/bin/ruff
```

---

_Comment by @Peiffap on 2025-08-19 15:59_

Thanks for working on this with me! I agree it's probably a good sign if the language server waits for input. That might've been a red herring for the issue with Zed. By the way, I'm trying to figure out the logging on Zed but it's not working the way I expect it to.
Your question about the ruff binary from the Zed log might explain this somewhat. Invoking the binary from the Zed log does not work, and I think any settings I pass to `ruff` are therefore ignored (which explains why I'm unable to set the `logLevel` to `"debug"`, IIUC). I'm not sure how to make sure the right ruff binary (i.e. the one in `which ruff`) is used.

What does (the relevant part of) your Zed `settings.json` look like?

---

_Comment by @ntBre on 2025-08-19 16:12_

No problem! I was a bit surprised that `ruff server` just crashed on unexpected input. I've never tried that either :)

I must admit I don't use Zed much, so it looks like I don't have any Ruff-specific config. My only LSP config is for rust-analyzer, which I think must have been default somehow:

```json
{
  "telemetry": {
    "metrics": false,
    "diagnostics": false
  },
  "vim_mode": true,
  "ui_font_size": 16,
  "buffer_font_size": 16,
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  },
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "cargo": {
          "features": "all"
        }
      }
    }
  }
}
```

This is my startup logging from Ruff in Zed's Log buffer:

```
2025-08-19T11:32:12-04:00 INFO  [project::lsp_store] attempting to start language server "ruff", path: "/home/brent/astral/ruff", id: 5
2025-08-19T11:32:12-04:00 INFO  [project::lsp_store] Refreshing workspace configurations for servers {}
2025-08-19T11:32:18-04:00 INFO  [lsp] starting language server process. binary path: "/home/brent/.local/share/zed/extensions/work/ruff/ruff-0.12.9/ruff-x86_64-unknown-linux-gnu/ruff", working directory: "/home/brent/astral/ruff", args: ["server"]
```

And invoking that binary directly does seem to work for me:

```shell
$ /home/brent/.local/share/zed/extensions/work/ruff/ruff-0.12.9/ruff-x86_64-unknown-linux-gnu/ruff --version
ruff 0.12.9
```

I also forgot to mention our [Zed](https://docs.astral.sh/ruff/editors/setup/#zed)-specific documentation, although I don't seen an option for setting the path there.

---

_Comment by @ntBre on 2025-08-19 16:17_

I also found this Zed discussion: https://github.com/zed-industries/zed/discussions/6669, which has a lot of different tips throughout the thread, but nothing that immediately stands out as relevant here.

---

_Comment by @MichaReiser on 2025-08-19 16:31_

> Running ruff server then pressing "Enter" gives me the following:

This is expected. The server uses stdin/stdout for the client/server communication. 

I do use Zed on a regular basis and haven't had any issues with ruff. 

> `/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/Users/gillespeiffer/.local/bin/ruff`

Can you try running `/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/Users/gillespeiffer/.local/bin/ruff version`


Can you also share your zed configuration

---

_Comment by @Peiffap on 2025-08-21 16:41_

> Can you try running `/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/Users/gillespeiffer/.local/bin/ruff version`

This file does not exist. `/Users/gillespeiffer/Library/Application Support/Zed/extensions/work/ruff/` only has 1 subdirectory path, `ruff-0.12.2/ruff-x86_64-apple-darwin/ruff` (`12.2` is not the version of `ruff` I am accessing from the terminal itself, but that's probably another issue still). The final `ruff` is just the executable.

---

> Can you also share your zed configuration

<details> 

```json
// Zed settings
//
// For information on how to configure Zed, see the Zed
// documentation: https://zed.dev/docs/configuring-zed
//
// To see all of Zed's default settings without changing your
// custom settings, run `zed: open default settings` from the
// command palette (cmd-shift-p / ctrl-shift-p)
{
  "features": {
    "edit_prediction_provider": "zed"
  },
  "edit_predictions": {
    "mode": "eager",
    "copilot": {
      "proxy": null,
      "proxy_no_verify": null,
      "enterprise_uri": null
    },
    "enabled_in_text_threads": false
  },
  "language_models": {
    "copilot_chat": {
      "api_url": "https://api.githubcopilot.com/chat/completions",
      "auth_url": "https://api.github.com/copilot_internal/v2/token",
      "models_url": "https://api.githubcopilot.com/models"
    }
  },
  "agent": {
    "model_parameters": [],
    "default_model": {
      "provider": "copilot_chat",
      "model": "gpt-4.1"
    }
  },
  "ui_font_size": 16,
  "buffer_font_size": 16,
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "Gruvbox Dark Soft"
  },
  "languages": {
    "TOML": {
      "format_on_save": "off"
    },
    "JSON": {
      "format_on_save": "off"
    },
    "Python": {
      "language_servers": ["pyright", "ruff"],
      "format_on_save": "on",
      "formatter": [
        {
          "language_server": {
            "name": "ruff"
          }
        }
      ]
    }
  },
  "lsp": {
    "pyright": {
      "settings": {
        "python.analysis": {
          "diagnosticMode": "workspace",
          "typeCheckingMode": "on"
        },
        "python": {
          "pythonPath": ".venv/bin/python"
        }
      }
    },
    "ruff": {
      "settings": {
        "logLevel": "debug",
        "logFile": "ruff.log"
      }
    }
  }
}
```

</details>

---

This appears to be some issue with where my `Zed` config is trying to find `ruff` as opposed to a problem with `ruff` itself. Happy to close the issue if the `ruff server` crash is expected behavior! Thanks guys.

---

_Comment by @MichaReiser on 2025-08-21 17:35_

> This appears to be some issue with where my Zed config is trying to find ruff as opposed to a problem with ruff itself. 

Yeah, it looks like the Zed extension failed to download Ruff but then still tries to run it? You could try deleting that directory and see if Zed redownloads Ruff

> Happy to close the issue if the ruff server crash is expected behavior! 

I'm not sure what you mean by the crash. Do you mean this

```
 ruff server

ruff failed
  Cause: Failed to start server
  Cause: disconnected channel
```

Yes, that's expected behavior (the server expects an open stdin/stdout channel)

---

_Comment by @Peiffap on 2025-08-21 21:24_

> Yeah, it looks like the Zed extension failed to download Ruff but then still tries to run it? You could try deleting that directory and see if Zed redownloads Ruff

It "downloads" an empty directory when I try to install the `ruff` extension from Zed after deleting.

> Do you mean this

Yes, apologies for not being clear!

---

Thanks guys!
I'll figure out the Zed issue on my own; let's see if a fresh install fixes things.

---

_Closed by @Peiffap on 2025-08-21 21:24_

---
