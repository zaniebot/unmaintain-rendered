```yaml
number: 1755
title: turn off diagnostics/typecheck diagnostics
type: issue
state: closed
author: carlesoctav
labels:
  - configuration
  - server
assignees: []
created_at: 2025-12-04T15:21:18Z
updated_at: 2026-01-05T21:44:31Z
url: https://github.com/astral-sh/ty/issues/1755
synced_at: 2026-01-12T15:54:25Z
```

# turn off diagnostics/typecheck diagnostics

---

_@carlesoctav_

### Question

TLDR: I want the reverse of [disableLanguageServices](https://docs.astral.sh/ty/reference/editor-settings/#disablelanguageservices)

My setup mainly uses Pyright + Ruff, with Pyright diagnostics disabled. I don’t really rely on full type-checking and only want simple diagnostics from the Ruff linter. I mostly use Pyright as a “go-to-definition” engine. I want do the same with ty. 

I’m trying to find a setting in ty that disables this while still keeping navigation features, but I couldn’t find anything equivalent.
 

### Version

_No response_

---

_Label `question` added by @carlesoctav on 2025-12-04 15:21_

---

_Label `question` removed by @MichaReiser on 2025-12-04 15:22_

---

_Label `configuration` added by @MichaReiser on 2025-12-04 15:22_

---

_Label `server` added by @MichaReiser on 2025-12-04 15:22_

---

_Comment by @MichaReiser on 2025-12-04 15:23_

I think adding this as a configuration option (or having a drop down with `LSP`, `Diagnostics only`, and `Full`) makes sense. 

An alternative is to allow users to easily deselect all rules. 

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-04 15:23_

---

_Comment by @echaya on 2025-12-05 12:50_

on nvim, it can be disabled by ` client.server_capabilities.diagnosticProvider = true`  but would be the best i can be controlled within ty setting

---

_Comment by @futurisold on 2025-12-19 09:09_

+1 for this. I'm having @carlesoctav's setup too: pyright (no diag) + ruff. This feature feels mandatory and obvious, especially since many of us already use ruff.

---

_Comment by @MichaReiser on 2025-12-19 09:12_

> +1 for this. I'm having @carlesoctav's setup too: pyright (no diag) + ruff. This feature feels mandatory and obvious, especially since many of us already use ruff.


Note that ty and ruff show different diagnostics. There are some diagnostics like syntax errors that are surfaced by both tools, but you can disable them in ruff https://docs.astral.sh/ruff/editors/settings/#showsyntaxerrors

---

_Comment by @futurisold on 2025-12-19 09:18_

I know, that is also true of pyright. In my case, I already have a configured ruff.toml with the rules I care about, and using ty comes down to the fact that it does not interfere at all with that setup. There is no point in having two tools. If ty provides unique diagnostics, they can become part of ruff, in my opinion, similar to how flake and others are integrated. What makes ty an exception to this rule?


---

_Comment by @MichaReiser on 2025-12-19 09:25_

> What makes ty an exception to this rule?

It works fundamentally differently from Ruff because it supports multifile analysis. Integrating ty (or ruff into ty) is a big lift. We may do this in the future but decided against it to ship a type checker sooner. 

---

_Comment by @futurisold on 2025-12-19 09:29_

That makes sense. Then I suppose the most convenient path forward is to give us an option to disable diagnostics.

---

_Closed by @MichaReiser on 2025-12-22 15:46_

---

_Comment by @samuelstevens on 2026-01-05 19:41_

How do I configure this setting in pyproject.toml? I do not use Zed/nvim/VSCode

---

_Comment by @carljm on 2026-01-05 19:50_

> How do I configure this setting in pyproject.toml? I do not use Zed/nvim/VSCode

I don't think we currently have any way to do this. Can you say more about your use case? The use case here was to use ty as a "go-to-definition" engine via LSP in an editor, so the config option that we added is an LSP config option.

If your use case is the same, but you use ty LSP via a different editor, it should still be possible to send the `diagnosticMode: off` LSP option, but the details depend on how your editor configures LSP servers.

All that said, I think we should probably add this option as a ty config option also? So that a project can set a general policy that ty is used in editors, but not for type errors.

---

_Comment by @samuelstevens on 2026-01-05 20:04_

I use Helix. Right now I configure servers with languages.toml

```toml
[language-server.ruff]
command = "uvx"
args = ["ruff", "server", "--preview"]

[language-server.ty]
command = "uvx"
args= ["ty", "server"]
config = { diagnosticMode = "off" }

[[language]]
name = "python"
language-servers = ["ruff", "ty"]
auto-format = true

```

But I don't see how to set `ty.diagnosticMode = 'off'` in languages.toml. Maybe this is a question for the Helix maintainers.

---

_Comment by @MichaReiser on 2026-01-05 20:19_

> All that said, I think we should probably add this option as a ty config option also? So that a project can set a general policy that ty is used in editors, but not for type errors.

I'm not convinced by this. Many editors have their own way of shared configurations. E.g. you can have a json config file checked in for VS Code for the shared config. It's also unclear to me what `ty check` is supposed to do if you disable diagnostics.

Looking at Ruff's helix documentation, the configuration should look like this

```toml
[language-server.ty.config]
diagnosticMode = "off"
```

or 

```toml
[language-server.ty.config.settings]
diagnosticMode = "off"
```

I'm not a 100% sure which one it is.

---

_Comment by @samuelstevens on 2026-01-05 21:44_

You're correct, both 

```toml
[language-server.ty]
command = "uvx"
args= ["ty", "server"]
config = { diagnosticMode = "off" }
```

and 

```toml
[language-server.ty.config]
diagnosticMode = "off"
```

work for me. Thanks for the help.

---
