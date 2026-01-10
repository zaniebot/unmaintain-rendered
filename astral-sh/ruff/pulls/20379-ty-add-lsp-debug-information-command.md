```yaml
number: 20379
title: "[ty] Add LSP debug information command"
type: pull_request
state: merged
author: Guillaume-Fgt
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: lsp_mem_use
created_at: 2025-09-14T18:20:13Z
updated_at: 2025-09-20T11:35:53Z
url: https://github.com/astral-sh/ruff/pull/20379
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add LSP debug information command

---

_Pull request opened by @Guillaume-Fgt on 2025-09-14 18:20_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

I implemented a Debug command for the ty_server in response to issue https://github.com/astral-sh/ty/issues/974.

I took inspiration from the Ruff server mechanism. The command can be executed with  `ty.printDebugInformation`. It accepts three different arguments: "short", "full" and "mypy_primer".

## Test Plan
I wrote tests for the command with the three different possible arguments. I also used it with Nvim with this configuration in my lua:

```
--command to trigger ty debug command for information about the session.
vim.api.nvim_create_autocmd('FileType', {
  pattern = 'python',
  callback = function()
    vim.keymap.set('n', '<leader>td', function()
      local clients = vim.lsp.get_active_clients({ bufnr = 0 })
      for _, client in ipairs(clients) do
        if client.name == "ty" then
          client.request("workspace/executeCommand", {
            command = "ty.printDebugInformation",
            arguments = {"short"}
          }, function(err, result)
            if err then
              vim.notify("Error: " .. err.message, vim.log.levels.ERROR)
            else
              vim.notify("Debug info: " .. vim.inspect(result), vim.log.levels.INFO)
            end
          end)
          break
        end
      end
    end, { buffer = true, desc = "Print Ty Debug Information" })
  end,
})
```

This is the result in action:


https://github.com/user-attachments/assets/07d9a070-86c8-4723-84d8-f082d966c99a




---

_Review requested from @carljm by @Guillaume-Fgt on 2025-09-14 18:20_

---

_Review requested from @MichaReiser by @Guillaume-Fgt on 2025-09-14 18:20_

---

_Review requested from @sharkdp by @Guillaume-Fgt on 2025-09-14 18:20_

---

_Review requested from @dcreager by @Guillaume-Fgt on 2025-09-14 18:20_

---

_Label `server` added by @AlexWaygood on 2025-09-14 19:27_

---

_Label `ty` added by @AlexWaygood on 2025-09-14 19:27_

---

_Comment by @github-actions[bot] on 2025-09-14 19:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-14 19:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review request for @sharkdp removed by @sharkdp on 2025-09-15 07:33_

---

_Comment by @MichaReiser on 2025-09-16 07:14_

I plan to look into this but I'm not sure if I get to it before the end of the week (I'm still catching up after my PTO)

---

_Assigned to @MichaReiser by @MichaReiser on 2025-09-16 07:31_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/execute_command.rs`:59 on 2025-09-19 07:50_

I think it's fine if this command only supports the `full` output format. It's a debug command and too much output isn't really an issue. 



---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/execute_command.rs`:45 on 2025-09-19 08:08_

It would be great to show the memory usage for all databases rather than just the first. I think there's also some additional information that's useful to show, like the resolved settings. 

This is untested, but maybe something like this

```rust
    let mut buffer = String::new();

    writeln!(buffer, "Client capabilities: {:#?}", session.client_capabilities());
    writeln!(buffer, "Position encoding: {:#?}", session.position_encoding());
    writeln!(buffer, "Global settings: {:#?}", session.global_settings());
    writeln!(buffer, "Open text documents: {}", session.text_document_keys().count());

    for (root, workspace) in session.workspaces() {
        writeln!(buffer, "Workspace {root} ({})", workspace.url())?;
        writeln!(buffer, "  Settings:\n\n{:#?}", workspace.settings())?;
        writeln!(buffer, "\n")?;
    }

    let mut first_db = true;

    for db in session.project_dbs() {
        writeln!(buffer, "\n")?;

        first_db = false;

        writeln!(buffer, "Project at {}", db.project().root(db))?;
        writeln!(buffer, "  Settings:\n{:#?}", db.project().settings(db))?;
        writeln!(buffer, "  Memory report:\n{}", db.salsa_memory_dump().display_full())?;
    }
```

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/commands.rs`:99 on 2025-09-19 08:15_

This is great that you added tests. 

I do think we'll need to redact the snapshot to prevent the test from becoming noisy. 

The easiest could be to simply omit the memory usage if `session.in_test` is true (you might need to expose the field with a `pub(crate)` method.

The alternative (and somewhat nicer) approach is to use insta's filter feature. We could add a filter that replaces all `[0-9]+.[0-9]+MB` with (`<X.XXMB>`) 





---

_@MichaReiser reviewed on 2025-09-19 08:17_

Thank you. I left a few comments on how we can make the command even more useful.

Are you also interested in contributing the necessary VS code changes:

1. Adding the command in the `package.json` ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/package.json#L447-L451))
2. Add a handler that automatically opens a window in VS code ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/src/common/commands.ts#L1-L124))

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-09-20 10:52_

---

_Renamed from "[ty] add mem usage report to LSP" to "[ty] Add LSP debug information command" by @MichaReiser on 2025-09-20 11:01_

---

_Comment by @MichaReiser on 2025-09-20 11:02_

This is great. Thank you!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-20 11:11_

---

_Merged by @MichaReiser on 2025-09-20 11:15_

---

_Closed by @MichaReiser on 2025-09-20 11:15_

---

_Comment by @Guillaume-Fgt on 2025-09-20 11:32_

Hi Micha, I was planning to make detailed explanations of my code modifications but my commits automatically triggered review apparently, I wasn't expected that. Thanks you for your comments on the code, it helped me a lot as I am still discovering the repo.

> Are you also interested in contributing the necessary VS code changes:
> 
>     1. Adding the command in the `package.json` ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/package.json#L447-L451))
> 
>     2. Add a handler that automatically opens a window in VS code ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/src/common/commands.ts#L1-L124))

I am less familiar with VS code but I am interested. I will work on it and open a PR or an issue if I need help 👍.



---

_Branch deleted on 2025-09-20 11:35_

---
