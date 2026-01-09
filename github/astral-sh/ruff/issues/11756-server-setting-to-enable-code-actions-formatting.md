---
number: 11756
title: Server setting to enable code actions / formatting on save
type: issue
state: open
author: DanCardin
labels:
  - configuration
  - server
assignees: []
created_at: 2024-06-05T15:03:03Z
updated_at: 2025-03-29T15:13:13Z
url: https://github.com/astral-sh/ruff/issues/11756
synced_at: 2026-01-07T13:12:15-06:00
---

# Server setting to enable code actions / formatting on save

---

_Issue opened by @DanCardin on 2024-06-05 15:03_

In the PR for adding the organizeImports code action, i saw a reference to:
```
  "[python]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.organizeImports": "explicit"
    },
    "editor.defaultFormatter": "charliermarsh.ruff"
  }
```

which, as far as i can tell, is a VSCode specific thing. Per https://github.com/astral-sh/ruff-lsp/issues/409 or https://github.com/astral-sh/ruff-lsp/issues/335, I'd love for there to be a `ruff server`-specific setting that optionally enables either/or both of `organizeImports` or `fixAll` actions on lsp format.

Personally, I'd likely enable just the `organizeImports`  on save (which i see no reason ever to not run, which is why i'd rather have it automatic than a code action i need to remember to perform), perhaps not fixAll.

---

_Label `server` added by @snowsignal on 2024-06-05 16:09_

---

_Comment by @snowsignal on 2024-06-05 17:59_

Thanks for opening this feature request, @DanCardin!

I do think this would a setting worth exploring for editors that don't have a way to run code actions / commands on save.

> I'd love for there to be a ruff server-specific setting that optionally enables either/or both of organizeImports or fixAll actions on lsp format.

Just to clarify - are you _also_ proposing that we introduce a separate setting to enable running `organizeImports` / `fixAll` when we format?

---

_Comment by @DanCardin on 2024-06-05 18:36_

Per my parenthesis block in the OP, i'd personally be happy with an `organizeImportsOnFormat = true` or `onFormat = {organizeImports = true}}` or whatever server setting (separate from the normal project-specific ruff config).

But with that said, I could imagine (by looking at https://github.com/astral-sh/ruff-lsp/issues/335) that **someone** might also want to enable `fixAll` on save too. They're essentially the same feature request. i'm just not personally invested in having both.

---

_Comment by @snowsignal on 2024-06-05 18:43_

Okay, thanks for clarifying 👍 

---

_Comment by @T-256 on 2024-06-08 00:22_

Another usecase at here is you always trigger format action manually and you didn't set code actions on save. I usually organize imports, format, and fix violations after I complemented the source of target module.
Currently I most use 3 actions each time I finished coding, it'd be nice to have three into one command.

## Proposal
### ruff server
Editor settings:
```jsonc
{
    "ruff.format.action": {
        "fix": true,
        "isort": true,
        "style": true,
    }
}
```
- By default, ONLY `style` enabled.
- `Format Document` will respect these settings, `Format Selection` ignores these settings.
- When `ruff.organizeImports` and/or `ruff.fixAll` editor settings are disabled, revealment enabled options don't have affect in format actions.
- Priority on which should be applied first when multi actions enabled: `isort > fix > style`. We could have separated settings for prioritization, but I think this is good default for now.

### ruff cli
The Ruff CLI needs further discussion around new command (e.g. `ruff action --fix --isort--no-style`).
Also, see this most-wanted feature request: https://github.com/astral-sh/ruff/issues/8232

---

_Referenced in [astral-sh/ruff#11803](../../astral-sh/ruff/pulls/11803.md) on 2024-06-08 16:16_

---

_Comment by @T-256 on 2025-03-01 12:16_

@DanCardin Any chances that this work for you?

> Thanks!  I was able to put this together with this keybind:
> ```
>  {
>     "command": "runCommands",
>     "key": "ctrl+shift+i",
>     "args": {
>       "commands": [
>         "editor.action.formatDocument",
>         "ruff.executeOrganizeImports"
>         // If you want autofix also - this won't let you specify a subset of rules (e.g. for only
>         // docstring format rules) until https://github.com/astral-sh/ruff/issues/12709
>         // "ruff.executeAutofix"
>       ]
>     },
>     "when": "editorLangId == python"
>   }
> ```
> which replaces the `Format Document` keybind for python, to run format + isort
> 
> _Originally posted by @aaron-skydio in https://github.com/astral-sh/ruff/discussions/15991#discussioncomment-12193766_

---

_Comment by @DanCardin on 2025-03-03 14:46_

I dont use vscode, which is why i'm looking for a generic lsp setting for this behavior.

With that said, it's not necessarily obvious to me why they're separate in the first place. The behavior I'm looking for is `ruff format`. So it **does** sort of feel odd that that's not the default behavior of the ruff lsp on format. Although i can certainly see the value in there **being** separate lsp actions for the standalone lint/organizeImports actions.

---

_Comment by @dhruvmanila on 2025-03-18 10:58_

The reason this is VS Code specific is because VS Code utilizes these capabilities that the server provides and exposes a setting to use them. Other editors require user to implement this in their configuration. For example, if you're using Neovim, you can update your config such that these code actions are run whenever you save the file.

We do want to provide a unified interface (https://github.com/astral-sh/ruff/issues/8232) but that requires a larger design discussion which involves looking at both the command-line and the server interface. Additional context: https://github.com/astral-sh/ruff/pull/11803#issuecomment-2162182083.

I believe the request here is not specific to code actions but it also involves formatting and applying the autofixes on save as well.

---

_Renamed from "`ruff server` setting enabling code actions on save" to "Server setting to enable code actions / formatting on save" by @dhruvmanila on 2025-03-18 11:00_

---

_Label `configuration` added by @dhruvmanila on 2025-03-18 11:00_

---

_Comment by @DanCardin on 2025-03-18 14:01_

yes and no (i think). I did originally request lint fixing as well, so that's the yes.

but the default format option does not organize imports, whereas `ruff format` both formats and organizes imports. even just making the save action (optionally or not) match what `ruff format` does, would be a step in the right direction imo. and **i think** that's besides the point of unifiying the lint/format interface

---

_Comment by @dhruvmanila on 2025-03-19 12:05_

> `ruff format` both formats and organizes imports.

This is not correct, the formatter does not organize your imports. It's the linter that organizes your import. In an editor context related to automation, it would be the code actions (`source.organizeImports`) that does it which in turn calls the linter. You can see this on the [playground] in the formatter tab that it's unchanged but opening the quick fix menu will provide a "Organize imports" action.

---

_Comment by @DanCardin on 2025-03-19 13:20_

ahhaaa (i was fooled by my own `make format` command which runs both 🤦). although i definitely would have expected isort to be a formatting concern rather than a linting one. certainly the line is fuzzy either way.

okay so i take it back, it is exactly about unifying the interface i guess. because while I am perfectly happy with individualized code actions for things like organizeImports, the point of format-on-save is to normalize the format such that I dont require any separate action to ensure my linter will only report "real" issues (which aren't unambiguously fixed formatting problems).

---

_Comment by @vanntile on 2025-03-29 15:13_

> For example, if you're using Neovim, you can update your config such that these code actions are run whenever you save the file.

@dhruvmanila how do you recommend that is done? For example, I am trying to run organizeImports (and later add the fixAll code action) on both a "format now" keymap and on save, and I see that this version does end up running it, in two steps:

```lua
vim.api.nvim_create_autocmd("BufWritePre", {
	pattern = "*.py",
	callback = function()
		vim.lsp.buf.code_action({
			context = {
				diagnostics = {},
				only = { "source.organizeImports" },
			},
			apply = true,
		})
		vim.lsp.buf.format({ async = false })
	end,
})
```

While using the following, for compatibility with the similar behavior from gopls and to make sure the code is run synchronously, does not run the action

```lua
if client and formatting_lsps[client.name] then
	local enc = client.offset_encoding or "utf-16"
	local params = vim.lsp.util.make_range_params(0, enc)
	params.context = { diagnostics = {}, only = { "source.organizeImports" } }

	local result = vim.lsp.buf_request_sync(0, "textDocument/codeAction", params, 1200)
	for _, res in pairs(result or {}) do
		for _, r in pairs(res.result or {}) do
			if r.edit then
				vim.lsp.util.apply_workspace_edit(r.edit, enc)
			end
		end
	end
	vim.lsp.buf.format({ async = false })
end
```

I am thinking it's possible the ruff organizeImports action does not take positional arguments, which makes something in the `buf_request_sync` fail.


---
