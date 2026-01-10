```yaml
number: 11805
title: Cannot setup ruff-lsp configuration in Helix config file
type: issue
state: closed
author: ondrej-ivanko
labels:
  - server
assignees: []
created_at: 2024-06-08T18:28:08Z
updated_at: 2024-06-10T18:24:56Z
url: https://github.com/astral-sh/ruff/issues/11805
synced_at: 2026-01-10T11:09:53Z
```

# Cannot setup ruff-lsp configuration in Helix config file

---

_Issue opened by @ondrej-ivanko on 2024-06-08 18:28_

Hi,

I tried to set up global settings for ruff-lsp/ruff. I am aware that such settings could be done with ruff.toml file in `~/.config/ruff/ruff.toml`. What I wanted to try is set up the same configuration in helix file languages.toml located in `~/.config/helix/languages.toml` as per documentation, it should be [possible](https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/setup/HELIX.md)

I tried changing `flake8-pytest-style` subsection

Neither this approach:
![Screenshot from 2024-06-08 20-15-34](https://github.com/astral-sh/ruff-lsp/assets/44980578/c2c17af7-8847-4e74-b95f-e2673f4b0e35)

Nor this have worked.
![Screenshot from 2024-06-08 20-16-20](https://github.com/astral-sh/ruff-lsp/assets/44980578/6148d09e-6f75-4363-87fa-639c4300e476)

It's just one example of single setting. The result is the same with other kind of settings in lint subsections.

Besides this issue, the lsp server works just fine.

I can enforce these settings by putting ruff.toml to project level directory with desired settings.

These kind of settings are not defined anywhere else. `Pyproject.toml` does not contain any ruff configuration. Ruff lsp is run only with `server --preview` args per documentation.

What's interesting is that err codes to be ignored defined in `[language-server.ruff.config.settings.lint]` are being actually registered and ignored by lsp, so I reckon it's only related to settings in subsections of `lint` key

By looking at helix logs, there is not issue with ruff server.

Version:
`ruff 0.4.8`
`ruff-lsp 0.0.53`
`helix 24.3 (44504b72)` - it's bleeding edge build

EDIT: The ruff-lsp does not seem to find or look for configuration file in `~/.config/ruff/ruff.toml`

I tried running ruff lsp with this config in helix config file:
```
[language-server.ruff]
command = "ruff"
args = ["server", "--preview", "--config", "/home/oji/.config/ruff/ruff.toml", "--isolated"]
```
 - the configuration file lint subsections were still ignored.

---

_Label `bug` added by @charliermarsh on 2024-06-08 20:36_

---

_Label `bug` removed by @charliermarsh on 2024-06-08 20:36_

---

_Comment by @charliermarsh on 2024-06-08 20:37_

Moving this to Ruff because it looks like you're using the native server (`ruff server`) rather than `ruff-lsp`.

---

_Label `server` added by @charliermarsh on 2024-06-08 20:37_

---

_Comment by @charliermarsh on 2024-06-08 20:38_

Can you share how you determined that "The ruff-lsp does not seem to find or look for configuration file in `~/.config/ruff/ruff.toml`"? Did you restart the server after adding that configuration file?

---

_Comment by @ondrej-ivanko on 2024-06-08 21:39_

Hi @charliermarsh, sry if there is any confusion. What I wrote is that the specific definitions for how ruff should treat certain "errors" is ignored when defined in subsections of `lint` key in the `~/.config/ruff/ruff.toml`.

It seems the ruff only uses certain keys from toml file to configure ruff server. When I set up changes in `[lint.flake8-annotations]` for example, it did not have any impact on what lint messages were displayed in the Helix IDE. Only when the same changes were made in `ruff.toml` on project level. This behaviour applied to all subsections of `lint` key that I tried.

I restarted server every time I made change to the configuration either by leaving the IDE or by restarting the lsp-server in IDE itself.

I was under the impression that `ruff server` start s`ruff-lsp` server.

---

_Assigned to @snowsignal by @snowsignal on 2024-06-10 13:49_

---

_Comment by @andrewcrook on 2024-06-10 16:00_

> I was under the impression that ruff server start sruff-lsp server.

Correct me if I am wrong but I think 

ruff-lsp is a third-party wrapper for ruff. Ruff didn’t have a lsp server originally.
ruff server --preview  was the  preview of the native (built in)  lsp server
I believe the native lsp is production ready and ruff-lsp isn't required anymore, unless it offers something different.

Personally I haven't gotten the native ruff lsp to work with neovim under lspconfig yet

On a side note. You have to be careful when you see server or daemon with type checks, linters and formatters because with some its an lsp interface and with others its running as a daemon for quicker start up times.
 

---

_Comment by @snowsignal on 2024-06-10 17:01_

> Personally I haven't gotten the native ruff lsp to work with neovim under lspconfig yet

@andrewcrook If this isn't too much to ask - could you file an issue describing what you're doing and what's going wrong? I'd like to help you figure this out.

---

_Comment by @charliermarsh on 2024-06-10 17:37_

> ruff-lsp is a third-party wrapper for ruff. Ruff didn’t have a lsp server originally.
> ruff server --preview was the preview of the native (built in) lsp server
> I believe the native lsp is production ready and ruff-lsp isn't required anymore, unless it offers something different.

Mostly right. `ruff-lsp` is an official Ruff thing (so it's "first-party" in that sense), but it is indeed a Python wrapper around the Ruff CLI that implements the Language Server Protocol. `ruff server` is the native, built-in LSP server. Our intent is to eventually sunset `ruff-lsp` (which we maintain) in favor of `ruff server`, but `ruff server` is currently in Beta.


---

_Comment by @snowsignal on 2024-06-10 18:02_

@ondrej-ivanko The reason why the `flake8-pytest-style` setting isn't working is because right now, we only expose a subset of Ruff's settings through the LSP settings. A list of the available settings can be found [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/MIGRATION.md#new-settings).

In this case, putting `flake8-pytest-style` in `~/.config/ruff/ruff.toml` is what you should be doing.

Also, you shouldn't have to pass `--config` to `ruff server` - we don't support this argument. instead, you can use the `configuration` setting like so:
```toml
[language-server.ruff.config.settings]
configuration = "~/.config/ruff/ruff.toml"
```

I hope this helps!

---

_Comment by @ondrej-ivanko on 2024-06-10 18:24_

Ok @snowsignal, all is clearer now. Thank you. I will keep ruff configured via ruff.toml for now. I'm closing this.

---

_Closed by @ondrej-ivanko on 2024-06-10 18:24_

---
