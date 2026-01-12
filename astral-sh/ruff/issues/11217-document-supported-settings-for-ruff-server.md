```yaml
number: 11217
title: "Document supported settings for `ruff server`"
type: issue
state: closed
author: snowsignal
labels:
  - documentation
  - server
assignees: []
created_at: 2024-04-30T17:15:07Z
updated_at: 2024-07-18T18:40:36Z
url: https://github.com/astral-sh/ruff/issues/11217
synced_at: 2026-01-12T15:54:50Z
```

# Document supported settings for `ruff server`

---

_@snowsignal_

Right now, we don't have any centralized documentation for `ruff server`'s client settings, similar to what [`ruff-lsp` has in its README](https://github.com/astral-sh/ruff-lsp?tab=readme-ov-file#settings). This would be a useful resource for users to have, especially for clients that require settings to be added manually in configuration.

---

_Label `documentation` added by @snowsignal on 2024-04-30 17:15_

---

_Label `server` added by @snowsignal on 2024-04-30 17:15_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-30 17:15_

---

_Comment by @charliermarsh on 2024-05-22 03:35_

Some of this in: https://github.com/astral-sh/ruff-vscode/pull/469

---

_Comment by @tartley on 2024-06-17 13:38_

Apologies for the uninformed drive-by, but as a user, I perceive the problem to be addressed here as two-fold:

1. As far as I can tell, the only current documentation of the available settings is in two places:
   1. The incidental mention on the [ruff-lsp migration guide](https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/MIGRATION.md). I do not know if this list is exhaustive, but my uninformed browsing of the source makes me think it is.
   2.  I see there have been recent merges which have refined a table of the available options [on the ruff-vscode project README](https://github.com/astral-sh/ruff-vscode/pull/469/commits/ae69083a78b2ba4e05c2e6531173765fbe3cd8e7) - but this is a bad place for it, people configuring other editors need this table too.

   This list should exist in somewhere more findable, more centrally applicable to all users of ruff server, not just those who are migrating from ruff-lsp or using VSCode.

4. For each of the editor specific guides, there needs to be an example which demonstrates how to provide the above settings in the initial lsp .setup() call (if that is actually the intent, I currently cannot tell.) For example, I'm looking at [the Neovim setup guide](https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/setup/NEOVIM.md), which:

    1. Links to nvim-lspconfig's #suggested-configuration, but this anchor does not exist in the linked document, nor does any similar-sounding title.
    3. Documents making the `require('lspconfig').ruff.setup {}` call without any args, which is fine.
    5. Links to the [#ruff entry in the nvim-lspserver project](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#ruff). This documents the default values to the setup() call to be (cmd, filetypes, root_dir, settings, single_file_support). I infer that the 'settings' table is where I should pass the settings documented in the ruff-lsp/ruff-vscode projects above. However, I have been unable to get any of those settings to work here. Do I pass them as:

       ```
       require('lspconfig').ruff.setup{ settings={ lineLength=100 } }
       ```

       I cannot get this to work, having tried many variations on it. But the following does work for me:

       ```
       require('lspconfig').ruff.setup{ settings={ args={ "--line-length=100" } } }
       ```
       even though "args" is not documented on the above linked pages, not on the linked [nvim-lspconfig entry](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#ruff), nor in the table of [ruff server settings](https://github.com/astral-sh/ruff-vscode/blob/7ef6eff8d6f760bb2861a96774b286f927152ad1/README.md#settings). Clearly I am misunderstanding. The docs should make this very clear.

I would send PRs, but I have no idea what I'm doing so wanted to at least chat about it first.

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-17 16:33_

---

_Comment by @dhruvmanila on 2024-06-18 06:03_

Hey @tartley, thank you for providing such a detailed analysis and sorry that you're facing this problem. @snowsignal can provide more details around it but I think the documentation for the new `ruff server` is in flux but we do plan to accumulate it in a central place like a "The Ruff Server" section at https://docs.astral.sh/.

I'll address a couple of points that you've mentioned here:

>   2. I see there have been recent merges which have refined a table of the available options [on the ruff-vscode project README](https://github.com/astral-sh/ruff-vscode/pull/469/commits/ae69083a78b2ba4e05c2e6531173765fbe3cd8e7) - but this is a bad place for it, people configuring other editors need this table too.

I think the settings provided in the linked table is specific to VS Code. Other editors need to configure it via other methods like `setup` function in Neovim and `language.toml` file for Helix. I think the goal of this issue is to address this need.

 
>   This list should exist in somewhere more findable, more centrally applicable to all users of ruff server, not just those who are migrating from ruff-lsp or using VSCode.
> * For each of the editor specific guides, there needs to be an example which demonstrates how to provide the above settings in the initial lsp .setup() call (if that is actually the intent, I currently cannot tell.)

Yes, I completely agree with this.
  
>   1. Links to nvim-lspconfig's #suggested-configuration, but this anchor does not exist in the linked document, nor does any similar-sounding title.

Hmm, let me fix that quickly. I don't think `nvim-lspconfig` now provides any suggestion on the configuration as it did earlier. I think I'll just provide a minimal config or link to some `:help` topic.

>   3. Links to the [#ruff entry in the nvim-lspserver project](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md?rgh-link-date=2024-06-17T13%3A38%3A16Z#ruff). This documents the default values to the setup() call to be (cmd, filetypes, root_dir, settings, single_file_support). I infer that the 'settings' table is where I should pass the settings documented in the ruff-lsp/ruff-vscode projects above. However, I have been unable to get any of those settings to work here. Do I pass them as:
>      ```
>      require('lspconfig').ruff.setup{ settings={ lineLength=100 } }
>      ```    
>        
>      I cannot get this to work, having tried many variations on it. But the following does work for me:
>      ```
>      require('lspconfig').ruff.setup{ settings={ args={ "--line-length=100" } } }
>      ```    
>        
>      even though "args" is not documented on the above linked pages, not on the linked [nvim-lspconfig entry](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md?rgh-link-date=2024-06-17T13%3A38%3A16Z#ruff), nor in the table of [ruff server settings](https://github.com/astral-sh/ruff-vscode/blob/7ef6eff8d6f760bb2861a96774b286f927152ad1/README.md#settings). Clearly I am misunderstanding. The docs should make this very clear.

I'm sorry that you had to face this but the solution to this is to provide it under `init_options`. So, your example would be:
```lua
require('lspconfig').ruff.setup {
  init_options = {
    settings = {
      -- Ruff server settings would go here
    }
  }
}
```

I know this isn't obvious and we're going to fix this in https://github.com/astral-sh/ruff/issues/11897 and I'll add an example section in NEOVIM.md file as well.

---

_Comment by @tartley on 2024-06-18 14:49_

@dhruvmanila Thank you for the detailed response! No apologies are necessary - my intent was not to complain, merely to try and help by being as explicit as I could about what I would like to see. Many thanks for all the amazing and exciting work to date, and to come!

---

_Comment by @minusf on 2024-07-03 14:48_

it seems that over time lspconfig maintainers moved away from a `suggested-configuration` and provide only a "minimal" configuration for bug reporting. I am sure they have their reasons but it makes it much harder to pick up the package for people like me. Also some of those handed down mappings have became hardcoded defaults in neovim 0.10+.

I have been playing the dictionary hunting game for finding where a setting is picked up, where it isn't for ages and it's very frustrating. I wish docs writers would stop using "..." and actually used an actual argument in their examples.

For example it took me a lot of trying to find out how to overwrite the default `cmd` for `ruff server`...  Is it in `init_options`? no.  Is it in `settings`?  no.  Is it in `settings` inside `init_options`? no.

```
lspconfig.ruff.setup {
  cmd = { "ruff", "server", "--preview", "--config", "~/Library/Application Support/ruff/pyproject.toml"},
}
```

My use case to override a project specified `pyproject.toml` file with my own as it has more checks than the ones the team agreed on...

While I can confirm with `LspInfo` that the command arguments are picked up, `ruff` still seems to favour the closest pyproject.toml file in the project directory, so now I am confused about the documentation: https://docs.astral.sh/ruff/configuration/#config-file-discovery point no 2...

```
Client: ruff (id: 2, bufnr: [1])
      filetypes:       python
      autostart:       true
 root directory:       ...
            cmd:       /opt/homebrew/bin/ruff server --preview --config ~/Library/Application Support/ruff/pyproject.toml
```

The new diagnostics show up only if I remove the project's `pyproject.toml` file. AAAnyway, I digressed a bit :D

---

_Comment by @minusf on 2024-07-03 14:56_

ah, turning on the trace messages told me at least to move the config file to `.config`.

---

_Comment by @minusf on 2024-07-03 15:23_

to give a positive update to my issue for anyone looking at a similar problem, `ruff-lsp` has honoured the settings correctly:

```
require('lspconfig').ruff_lsp.setup {
  init_options = {
    settings = {
      -- Any extra CLI arguments for `ruff` go here.
      args = {"--config", "~/.config/ruff/pyproject.toml"},
    }
  }
}
```

so back to `ruff-lsp` for a little while.

---

_Comment by @dhruvmanila on 2024-07-04 05:03_

@minusf Sorry that you're facing this, I agree that it's very frustrating to find where a specific information goes with the amount of options available. I'm going to be working on the new `ruff server` documentation next week and this is helpful context to guide in how I should go about writing the examples. Thank you for sharing this.

---

_Comment by @minusf on 2024-07-04 08:44_

To be clear, I think this is an lspconfig issue first, other projects later.  They just kind of expect everyone to be a lua dev 🤣 

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-16 09:49_

---

_Unassigned @snowsignal by @dhruvmanila on 2024-07-16 09:49_

---

_Closed by @dhruvmanila on 2024-07-18 12:11_

---

_Closed by @dhruvmanila on 2024-07-18 12:11_

---

_Comment by @dhruvmanila on 2024-07-18 18:35_

Thank you all for providing feedback here! It was helpful while writing the documentation. Feel free to provide any additional suggestions that you might have in the new documentation. Reference: https://docs.astral.sh/ruff/editors

---

_Comment by @tartley on 2024-07-18 18:40_

Yes, I've been poring through it right now. I'll let you know and could work on modest PRs if I spot anything.

On Thu, Jul 18, 2024, at 12:36, Dhruv Manilawala wrote:
> 
> 
> Thank you all for providing feedback here! It was helpful while writing the documentation. Feel free to provide any additional suggestions that you might have in the new documentation. Reference: https://docs.astral.sh/ruff/editors
> 
> 
> —
> Reply to this email directly, view it on GitHub <https://github.com/astral-sh/ruff/issues/11217#issuecomment-2237247445>, or unsubscribe <https://github.com/notifications/unsubscribe-auth/AABBV66D3YMTUCDIME7GBR3ZNADJVAVCNFSM6AAAAABHAVNULKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDEMZXGI2DONBUGU>.
> You are receiving this because you were mentioned.Message ID: ***@***.***>
> 

--
Jonathan Hartley         USA, Central(UTC-5)
***@***.***  https://tartley.com


---
