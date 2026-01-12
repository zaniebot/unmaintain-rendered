```yaml
number: 16214
title: Update server to return the debug info as string
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/print-debug-info-return-string
created_at: 2025-02-17T15:42:02Z
updated_at: 2025-02-18T09:46:45Z
url: https://github.com/astral-sh/ruff/pull/16214
synced_at: 2026-01-12T15:55:54Z
```

# Update server to return the debug info as string

---

_@dhruvmanila_

## Summary

This PR updates the `ruff.printDebugInformation` command to return the info as string in the response. Currently, we send a `window/logMessage` request with the info but that has the disadvantage that it's not visible to the user directly.

What `rust-analyzer` does with it's `rust-analyzer/status` request which returns it as a string which then the client can just display it in a separate window. This is what I'm thinking of doing as well.

Other editors can also benefit from it by directly opening a temporary file with this information that the user can see directly.

There are couple of options here:
1. Keep using the command, keep the log request and return the string
2. Keep using the command, remove the log request and return the string
3. Create a new request similar to `rust-analyzer/status` which returns a string

This PR implements (1) but I'd want to move towards (2) and remove the log request completely. We haven't advertised it as such so this would only require updating the VS Code extension to handle it by opening a new document with the debug content.

## Test plan

For VS Code, refer to https://github.com/astral-sh/ruff-vscode/pull/694.

For Neovim, one could do:
```lua
local function execute_ruff_command(command)
  local client = vim.lsp.get_clients({ 
    bufnr = vim.api.nvim_get_current_buf(), 
    name = name,
    method = 'workspace/executeCommand',
  })[1]
  if not client then
    return
  end
  client.request('workspace/executeCommand', {
    command = command,
    arguments = {
      { uri = vim.uri_from_bufnr(0) }
    },
    function(err, result)
      if err then
        -- log error
        return
      end
      vim.print(result)
      -- Or, open a new window with the `result` content
    end
  }
```

---

_Label `server` added by @dhruvmanila on 2025-02-17 15:42_

---

_Comment by @github-actions[bot] on 2025-02-17 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2025-02-18 04:34_

I prefer to remove the logging part and I don't think it should break anything.

---

_@dhruvmanila reviewed on 2025-02-18 04:34_

---

_Marked ready for review by @dhruvmanila on 2025-02-18 04:34_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-18 04:34_

---

_@MichaReiser reviewed on 2025-02-18 07:51_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2025-02-18 07:51_

I'm in favor of removing it, but wouldn't it break old VS code clients that don't have the required code to open the document on the client side? 

---

_@MichaReiser reviewed on 2025-02-18 07:52_

Could you add a test plan showing how this works for clients without an editor extension (e.g. neovim)? How does the client know that it has to open a document? Is this the default behavior of a custom command?

---

_Comment by @dhruvmanila on 2025-02-18 08:44_

> Could you add a test plan showing how this works for clients without an editor extension (e.g. neovim)? How does the client know that it has to open a document? Is this the default behavior of a custom command?

We haven't documented the commands API anywhere nor is it easy to add the argument schema in the capabilities as the arguments are `[]LspAny` (https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#executeCommandParams).

No, this is not the default behavior for a custom command. The user or the plugin author needs to know the argument schema for each command to implement it. As to how to pass in the document URI, that will be dependent on each editor. For example, in VS Code, it's just a `vscode.window.activeTextEditor` while in Neovim it would be [`vim.lsp.util.make_text_document_params`](https://neovim.io/doc/user/lsp.html#vim.lsp.util.make_text_document_params()).

Hmm, that makes me wonder why weren't we using the existing types from `lsp_types` like [`VersionedTextDocumentIdentifier`](https://docs.rs/lsp-types/0.95.1/lsp_types/struct.VersionedTextDocumentIdentifier.html) instead of `Argument`. I'll make this as a follow-up change.

---

_@dhruvmanila reviewed on 2025-02-18 08:47_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2025-02-18 08:47_

Hmm yeah. So, that would happen if the user is using a non-latest extension version with the latest ruff in which case I think we should recommend them to upgrade the extension version as it always include improvements and bug fixes. I'm fine keeping this here as well for the time being and we can remove it with 0.10.

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-18 09:14_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2025-02-18 09:34_

Can we include a `TODO` or similar that it should be removed? Or do we want to keep it, considering that other clients can't easily call commands? 

---

_@MichaReiser approved on 2025-02-18 09:34_

---

_@dhruvmanila reviewed on 2025-02-18 09:37_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2025-02-18 09:37_

I think we should remove it in a later version, I'll create an issue for 0.10.

---

_Merged by @dhruvmanila on 2025-02-18 09:46_

---

_Closed by @dhruvmanila on 2025-02-18 09:46_

---

_Branch deleted on 2025-02-18 09:46_

---
