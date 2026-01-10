```yaml
number: 11850
title: Add supported commands in server capabilities
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-command
created_at: 2024-06-13T03:13:31Z
updated_at: 2024-06-13T04:02:44Z
url: https://github.com/astral-sh/ruff/pull/11850
synced_at: 2026-01-10T21:56:00Z
```

# Add supported commands in server capabilities

---

_Pull request opened by @dhruvmanila on 2024-06-13 03:13_

## Summary

This PR updates the server capabilities to include the commands that Ruff supports. This is similar to how there's a list of possible code actions supported by the server.

I noticed this when I was trying to find whether Helix supported workspace commands or not based on Jane's comment (https://github.com/astral-sh/ruff/pull/11831#discussion_r1634984921) and I found the `:lsp-workspace-command` in the editor but it didn't show up anything in the picker.

So, I looked at the implementation in Helix (https://github.com/helix-editor/helix/blob/9c479e6d2de3bca9dec304f9182cee2b1c0ad766/helix-term/src/commands/typed.rs#L1372-L1384) which made me realize that Ruff doesn't provide this in its capabilities. Currently, this does require `ruff` to be first in the list of language servers in the user config but that should be resolved by https://github.com/helix-editor/helix/pull/10176. So, the following config should work:

```toml
[[language]]
name = "python"
# Ruff should come first until https://github.com/helix-editor/helix/pull/10176 is released
language-servers = ["ruff", "pyright"]
```

## Test Plan

1. Neovim's server capabilities output should include the supported commands:

```
  executeCommandProvider = {                                                                                                                          
    commands = { "ruff.applyFormat", "ruff.applyAutofix", "ruff.applyOrganizeImports", "ruff.printDebugInformation" },                                
    workDoneProgress = false                                                                                                                          
  },
```

2. Helix should now display the commands to pick from when `:lsp-workspace-command` is invoked:

<img width="832" alt="Screenshot 2024-06-13 at 08 47 14" src="https://github.com/astral-sh/ruff/assets/67177269/09048ecd-c974-4e09-ab56-9482ff3d780b">


---

_Label `server` added by @dhruvmanila on 2024-06-13 03:13_

---

_Review requested from @snowsignal by @dhruvmanila on 2024-06-13 03:13_

---

_Comment by @dhruvmanila on 2024-06-13 03:16_

I think I'll re-order them to display the "printDebugInformation" at the end.

Edit: Done

---

_Comment by @github-actions[bot] on 2024-06-13 03:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-06-13 03:43_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:373 on 2024-06-13 03:43_

I renamed this from `Command` to `SupportedCommand` to match `SupportedCodeAction` and also moved it from the `execute_command.rs` module. Feel free to disagree on anything.

---

_@snowsignal approved on 2024-06-13 03:57_

LGTM! 

It's unfortunate that Helix isn't able to forward the URI for these commands - maybe that's something we should try contributing upstream eventually.

---

_Comment by @dhruvmanila on 2024-06-13 04:02_

> It's unfortunate that Helix isn't able to forward the URI for these commands - maybe that's something we should try contributing upstream eventually.

Yeah, we can start by opening an issue / discussion to let them know about this limitation. I also think this can be solved by a custom plugin but I don't think Helix supports them yet.

---

_Merged by @dhruvmanila on 2024-06-13 04:02_

---

_Closed by @dhruvmanila on 2024-06-13 04:02_

---

_Branch deleted on 2024-06-13 04:02_

---
