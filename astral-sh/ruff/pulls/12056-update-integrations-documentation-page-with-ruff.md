```yaml
number: 12056
title: "Update Integrations documentation page with `ruff server` explanation and setup guides"
type: pull_request
state: closed
author: snowsignal
labels:
  - documentation
  - server
assignees: []
base: main
head: jane/docs/update-integrations
created_at: 2024-06-26T22:31:25Z
updated_at: 2024-07-16T09:53:02Z
url: https://github.com/astral-sh/ruff/pull/12056
synced_at: 2026-01-12T15:55:40Z
```

# Update Integrations documentation page with `ruff server` explanation and setup guides

---

_@snowsignal_

_No description provided._

---

_Label `documentation` added by @snowsignal on 2024-06-26 22:31_

---

_Label `server` added by @snowsignal on 2024-06-26 22:31_

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-26 22:31_

---

_Renamed from "Update `integrations.md` with `ruff server` explanation and setup guides" to "Update Integrations documentation page with `ruff server` explanation and setup guides" by @snowsignal on 2024-06-26 22:32_

---

_Review requested from @charliermarsh by @snowsignal on 2024-06-26 22:33_

---

_Comment by @charliermarsh on 2024-06-28 01:11_

A few questions before I dig into the copy:

1. As mentioned in Discord, should we move any `ruff-lsp` documentation that doesn't already exist in the `ruff-lsp` README to that README, so we're not losing anything useful?

2. Should this be replacing the setup guides and documentation that you have in the `ruff-server` crate? If not, why not? What's the difference?

---

_Comment by @snowsignal on 2024-06-28 01:34_

@charliermarsh 
> 1. As mentioned in Discord, should we move any ruff-lsp documentation that doesn't already exist in the ruff-lsp README to that README, so we're not losing anything useful?

We should, though I plan to also keep around the deprecated documentation in the Integration page for a little while longer under its own section.

> 2. Should this be replacing the setup guides and documentation that you have in the ruff-server crate? If not, why not? What's the difference?

We could probably remove the setup guides, yes. Those have just been copy-pasted.

---

_Comment by @MichaReiser on 2024-06-28 07:17_

It seems that our documentation doesn't support more than one top-level heading per page. 

![image](https://github.com/astral-sh/ruff/assets/1203881/783a81b5-f761-4335-8d6d-e6cec776bb97)

* The navigation entry is *Integrations* but the page title is *Editor Integrations*
* *Editor Integrations* doesn't show up in the navigation
* The entire *Other integrations* sub menu is missing


Can you try keeping *Integrations* as the page header and use `##` for *Editor integrations* and *Other integrations* (and reduce the heading of all other sub-headings). 


We should probably also hide the sub-headers in the neovim section from the navigation.

---

_@MichaReiser reviewed on 2024-06-28 07:19_

---

_Review comment by @MichaReiser on `docs/integrations.md`:6 on 2024-06-28 07:19_

I would remove the mention of `ruff-lsp` from here. I worry that it is more confusing than helpful for users that just want to set up their IDE. 

We could even argue that for most users, the entire *Ruff Language Server* section isn't relevant if, all they want to do, is set up their VS code.

---

_@snowsignal reviewed on 2024-06-28 10:27_

---

_Review comment by @snowsignal on `docs/integrations.md`:6 on 2024-06-28 10:27_

I think the added context about the language server would be helpful for users using editors besides VS Code. I'm open to removing the mention of `ruff-lsp` here, but I think we should still document it in its deprecated state.

---

_@charliermarsh reviewed on 2024-06-28 12:33_

---

_Review comment by @charliermarsh on `docs/integrations.md`:5 on 2024-06-28 12:33_

```suggestion
The Ruff language server, also known as `ruff server`, powers the diagnostic and formatting capabilities of Ruff's VS Code extension and other editor integrations. It is a single, common backend built directly into Ruff, and a direct replacement for `ruff-lsp`, our previous language server. You can read more about `ruff server` in the [`v0.4.5` blog post](https://astral.sh/blog/ruff-v0.4.5).
```

---

_@charliermarsh reviewed on 2024-06-28 12:33_

---

_Review comment by @charliermarsh on `docs/integrations.md`:7 on 2024-06-28 12:33_

```suggestion
`ruff server` uses the LSP (Language Server Protocol), and as such works with any editor that supports this protocol. VS Code, Neovim, Helix, and Kate all have official, documented support for `ruff server`. Other editors may support the legacy `ruff-lsp` server; see the [`ruff-lsp` documentation](https://github.com/astral-sh/ruff-lsp#setup) for more.
```

---

_@charliermarsh reviewed on 2024-06-28 12:34_

---

_Review comment by @charliermarsh on `docs/integrations.md`:11 on 2024-06-28 12:34_

```suggestion
Download the [Ruff VS Code extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff), which supports code formatting, fix actions, import sorting, and more.
```

---

_Review comment by @charliermarsh on `docs/integrations.md`:13 on 2024-06-28 12:34_

```suggestion
By default, the extension will attempt to use the `ruff` executable installed on your local system, and will use a bundled executable if that's not available. If `ruff` is `v0.4.5` or later, the extension will automatically use `ruff server`; otherwise, it will use fall back to `ruff-lsp`.
```

---

_@charliermarsh reviewed on 2024-06-28 12:34_

---

_@MichaReiser reviewed on 2024-06-28 12:34_

---

_Review comment by @MichaReiser on `docs/integrations.md`:6 on 2024-06-28 12:34_

That makes sense. To me, the mentioning in

> Other editors may support the legacy `ruff-lsp` server; see the [`ruff-lsp` documentation](https://github.com/astral-sh/ruff-lsp#setup) for more.

seems sufficient.

---

_@charliermarsh reviewed on 2024-06-28 12:36_

---

_Review comment by @charliermarsh on `docs/integrations.md`:15 on 2024-06-28 12:36_

```suggestion
To configure Ruff to format, fix, and organize imports on-save, add the following to your `settings.json`:

```json
{
  "[python]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.organizeImports": "explicit"
    },
    "editor.defaultFormatter": "charliermarsh.ruff"
  }
}
```

For more on configuring the extension, refer to the [`ruff-vscode` documentation](https://github.com/astral-sh/ruff-vscode/?tab=readme-ov-file#configuring-ruff).
```

---

_Review comment by @charliermarsh on `docs/integrations.md`:74 on 2024-06-28 12:37_

```suggestion
To enable logging in Neovim, set the `RUFF_TRACE` environment variable
```

---

_@charliermarsh reviewed on 2024-06-28 12:37_

---

_@charliermarsh reviewed on 2024-06-28 12:38_

---

_Review comment by @charliermarsh on `docs/integrations.md`:83 on 2024-06-28 12:38_

```suggestion
You can then set the desired log level (e.g., `debug` vs. `warning`) in the extension `settings`:
```

---

_@charliermarsh reviewed on 2024-06-28 12:39_

---

_Review comment by @charliermarsh on `docs/integrations.md`:125 on 2024-06-28 12:39_

```suggestion
Next, register the language server for use in Python files.
```

---

_Comment by @charliermarsh on 2024-06-28 12:40_

I think we should consider documenting all of the supported LSP settings here (it could be similar to what we have in the VS Code README). As-is, I don't _think_ that documentation is available anywhere, or is it?

---

_Comment by @snowsignal on 2024-06-28 12:53_

@charliermarsh We cover the settings that were added/remove in the [migration guide](https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/MIGRATION.md), but it isn't a comprehensive list. I agree that having a complete settings guide would make sense for this document.

---

_Comment by @charliermarsh on 2024-06-28 12:55_

Should the migration guide also be part of the published documentation?

---

_Comment by @snowsignal on 2024-06-28 13:09_

I'd rather link to it than add it to the documentation itself.

---

_Comment by @dhruvmanila on 2024-07-16 09:52_

Closing in favor of #12344 

---

_Closed by @dhruvmanila on 2024-07-16 09:52_

---

_Removed from milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-16 09:53_

---
