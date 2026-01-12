```yaml
number: 12624
title: "[red-knot] Implement basic LSP server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/red-knot-lsp-1
created_at: 2024-08-02T09:39:52Z
updated_at: 2024-08-06T11:41:46Z
url: https://github.com/astral-sh/ruff/pull/12624
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Implement basic LSP server

---

_@dhruvmanila_

## Summary

This PR adds basic LSP implementation for the Red Knot project.

This is basically a fork of the existing `ruff_server` crate into a `red_knot_server` crate. The following are the main differences:
1. The `Session` stores a map from workspace root to the corresponding Red Knot database (`RootDatabase`).
2. The database is initialized with the newly implemented `LSPSystem` (implementation of `System` trait)
3. The `LSPSystem` contains the server index corresponding to each workspace and an underlying OS system implementation. For certain methods, the system first checks if there's an open document in LSP system and returns the information from that. Otherwise, it falls back to the OS system to get that information. These methods are `path_metadata`, `read_to_string` and `read_to_notebook`
4. Add `as_any_mut` method for `System`

**Why fork?**

Forking allows us to experiment with the functionalities that are specific to Red Knot. The architecture is completely different and so the requirements for an LSP implementation are different as well. For example, Red Knot only supports a single workspace, so the LSP system needs to map the multi-workspace support to each Red Knot instance. In the end, the server code isn't too big, it will be easier to implement Red Knot specific functionality without worrying about existing server limitations and it shouldn't be difficult to port the existing server.

## Review

Most of the server files hasn't been changed. I'm going to list down the files that have been changed along with highlight the specific part of the file that's changed from the existing server code.

Changed files:
* Red Knot CLI implementation: https://github.com/astral-sh/ruff/pull/12624/files#diff-579596339a29d3212a641232e674778c339b446de33b890c7fdad905b5eb50e1
* In https://github.com/astral-sh/ruff/pull/12624/files#diff-b9a9041a8a2bace014bf3687c3ef0512f25e0541f112fad6131b14242f408db6, server capabilities have been updated, dynamic capability registration is removed
* In https://github.com/astral-sh/ruff/pull/12624/files#diff-b9a9041a8a2bace014bf3687c3ef0512f25e0541f112fad6131b14242f408db6, the API for `clear_diagnostics` now take in a `Url` instead of `DocumentQuery` as the document version doesn't matter when clearing diagnostics after a document is closed
* [`did_close`](https://github.com/astral-sh/ruff/pull/12624/files#diff-9271370102a6f3be8defaca40c82485b0048731942520b491a3bdd2ee0e25493), [`did_close_notebook`](https://github.com/astral-sh/ruff/pull/12624/files#diff-96fb53ffb12c1694356e17313e4bb37b3f0931e887878b5d7c896c19ff60283b), [`did_open`](https://github.com/astral-sh/ruff/pull/12624/files#diff-60e852cf1aa771e993131cabf98eb4c467963a8328f10eccdb43b3e8f0f1fb12), [`did_open_notebook`](https://github.com/astral-sh/ruff/pull/12624/files#diff-ac356eb5e36c3b2c1c135eda9dfbcab5c12574d1cb77c71f7da8dbcfcfb2d2f1) are updated to open / close file from the corresponding Red Knot workspace
* The [diagnostic handler](https://github.com/astral-sh/ruff/pull/12624/files#diff-4475f318fd0290d0292834569a7df5699debdcc0a453b411b8c3d329f1b879d9) is updated to request diagnostics from Red Knot
* The [`Session::new`] method in https://github.com/astral-sh/ruff/pull/12624/files#diff-55c96201296200c1cab37c8b0407b6c733381374b94be7ae50563bfe95264e4d is updated to construct the Red Knot databases for each workspace. It also contains the `index_mut` and `MutIndexGuard` implementation
* And, `LSPSystem` implementation is in https://github.com/astral-sh/ruff/pull/12624/files#diff-4ed62bd359c43b0bf1a13f04349dcd954966934bb8d544de7813f974182b489e

## Test Plan

First, configure VS Code to use the `red_knot` binary

1. Build the `red_knot` binary by `cargo build`
2. Update the VS Code extension to specify the path to this binary
```json
{
	"ruff.path": ["/path/to/ruff/target/debug/red_knot"]
}
```
3. Restart VS Code

Now, open a file containing red-knot specific diagnostics, close the file and validate that diagnostics disappear.

---

_Label `red-knot` added by @dhruvmanila on 2024-08-02 09:39_

---

_Comment by @github-actions[bot] on 2024-08-04 09:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @dhruvmanila on 2024-08-04 09:58_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-04 09:58_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-04 09:58_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-04 09:58_

---

_@MichaReiser reviewed on 2024-08-05 08:21_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:3 on 2024-08-05 08:21_

I prefer keeping the version set to `0.0.0` or `0.0.1` until we make our first release. 

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server.rs`:168 on 2024-08-05 08:25_

Would you mind incorporating the changes from https://github.com/astral-sh/ruff/pull/12610

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session.rs`:36 on 2024-08-05 08:26_

We may want to add a short comment explaining why this is an option and when it can be `None`

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session.rs`:69 on 2024-08-05 08:27_

Do we need a different system for each workspace or could we use a single, shared system for all of them (where `LSPSystem` itself is cheap cloneable?)

If so, then we could maybe even store the `system` on the `Session` so that we don't need to loop over all workspaces and take the index from each `LSPSystem`. Instead, we can just take the `Index` from the system stored on `index` (which is the one shared with all workspaces.)

This may even remove the need for `system_mut` (and `as_any_mut`)

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:34 on 2024-08-05 08:32_

Same as for `Session`. I recommend to add a note when `index` can be `None`.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:119 on 2024-08-05 08:34_

We may want to query the os file system to retrieve the permissions on disk.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:132 on 2024-08-05 08:37_

This block seems a bit repetitive. I think I would change `system_path_to_document_ref` to return `Result<Option>`. So that this can be simplified to

```rust
let document = self.system_path_to_document_ref(path)?; 

if let Some(document) = document {
    if let DocumentQuery::Text { document, .. } = &document {
        Ok(documents.contents().to_string())
    } else {
        Err(...)
    }
} else {
    self.os_system.read_to_string(path)
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:144 on 2024-08-05 08:39_

What's the motivation of erroring out when the document is a notebook? We would have to mention it in the `System` contract if calling `read_to_string` isn't allowed for notebooks).

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:155 on 2024-08-05 08:40_

Nit: Maybe move the cast to a function so that you only have to document the "safety" once

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:160 on 2024-08-05 08:43_

Similar to `read_to_string`. What if a file does have the `ipynb` extension but the editor (e.g. neovim) didn't open it as a notebook? I think we should just fall back to calling `Notebook::from_source_code`. 

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/lib.rs`:24 on 2024-08-05 08:43_

Use the red knot or the server crate version?

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session/settings.rs`:34 on 2024-08-05 08:45_

I'm not sure if we should remove the `lint` and `format` settings for now.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session/index.rs`:26 on 2024-08-05 08:48_

We probably have to re-work how settings work because we need to pass them somehow to red knot. I'm leaning towards removing most/all setting support for now.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/diagnostic.rs`:53 on 2024-08-05 08:53_

I would prefer if the logic to go from `url` to `file` is hidden a way. Maybe a method on `snapshot.file()`? 

We may even want to store the `File` alongside with the document to avoid unnecessary `system_path_to_file` calls. 

Overall, calling `system_path_to_file` (or any other method not exposed on `db`) is "dangerous" becaues they may panic at any point when the database gets mutated. That's why we should avoid calling into any methods not exposed on `RootDatabase`.

---

_@dhruvmanila reviewed on 2024-08-05 08:53_

---

_Review comment by @dhruvmanila on `crates/red_knot/Cargo.toml`:3 on 2024-08-05 08:53_

The only reason I added this was to simplify the VS Code setup because it wouldn't start the server unless it's 0.3.5

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_open_notebook.rs`:45 on 2024-08-05 08:58_

We only need to call `open_file` if you want that `workspace.check` only checks the open files and not all files in the workspace. 

This method can also possibiliy panic if another thread calls `get_mut` first. I guess that's "impossible" because `open_notebook_document` already aquires a lock on `Db`, but that's rather subtle. I'm leaning towards removing this call for now. We probably want to add a `RootDatabase.open_path` method or similar long term (and maybe this should happen in `open_notebook_dcument`?)

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_open.rs`:37 on 2024-08-05 08:58_

Same as for `did_open_notebook`

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_close.rs`:40 on 2024-08-05 08:59_

Same as for `did_open_notebook`. I would remove this for now. 

Do we need to inform the `RootDatabase` that the file migt have changed? E.g. in the case where there are unsaved changes that get discarded?

---

_@MichaReiser reviewed on 2024-08-05 09:03_

This overall looks great. We may not have to add `system_mut` after all. I added an inline comment for that. 

The other two things that we have to figure out are but I would recommend removing for now:

* Settings
* API exposed to the LSP: Calling `system_path_to_file` can panic if any other thread calls `Handle::get_mut`. I think the LSP should instead use a facade with a limited set of queries that wrapps the queries in `RootDatabase::with_db`, similar to rust analyzer. Any queries not exposed on the facade should be considered internal. 

---

_@MichaReiser reviewed on 2024-08-05 09:04_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:3 on 2024-08-05 09:04_

Hmm, can we change the extension to skip the check if the binary's name is red_knot?

---

_Review comment by @dhruvmanila on `crates/red_knot/Cargo.toml`:3 on 2024-08-05 11:56_

Can do!

---

_@dhruvmanila reviewed on 2024-08-05 11:56_

---

_@dhruvmanila reviewed on 2024-08-05 12:33_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session.rs`:69 on 2024-08-05 12:33_

> Do we need a different system for each workspace or could we use a single, shared system for all of them (where `LSPSystem` itself is cheap cloneable?)

Just to confirm, you mean just the system and not the root database as a whole, right?

I think that should be possible, we already use a global index to store the open documents from all workspaces. This would mean that we store the `Index` inside the `LSPSystem` itself and both the `Session` and `RootDatabase` accesses this instead.

---

_@dhruvmanila reviewed on 2024-08-05 12:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:132 on 2024-08-05 12:40_

I'm not sure how this could work because `system_path_to_document_ref` could give two errors, (1) invalid input because system path to url conversion failed or (2) no document exists for the url. We should only fall back to the OS system in case it's (2).

If we convert `system_path_to_document_ref` to return an `Option`, then we can't tell the difference between the two.

---

_@dhruvmanila reviewed on 2024-08-05 12:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:144 on 2024-08-05 12:47_

I think my main motivation was coming from my understanding of `read_to_string` where I thought it's the caller's responsibility to make sure that this function is used to only read a Python file and not a Notebook file. I think that might be incorrect. We might have to serialize the notebook to a JSON string in this case.

---

_@MichaReiser reviewed on 2024-08-05 12:47_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session.rs`:69 on 2024-08-05 12:47_

> Just to confirm, you mean just the system and not the root database as a whole, right?

Yes

> This would mean that we store the Index inside the LSPSystem itself and both the Session and RootDatabase accesses this instead.

That makes sense. It makes me wonder if we could implement `System` on `Index` instead.

---

_@MichaReiser reviewed on 2024-08-05 12:48_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:132 on 2024-08-05 12:48_

Sorry. I missed the second \` in the first sentence and it resulted in the sentence being truncated. I would return a `Result<Option<Document>>`

---

_@dhruvmanila reviewed on 2024-08-05 12:50_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:160 on 2024-08-05 12:50_

Yeah, I think this makes sense in this case.

---

_@dhruvmanila reviewed on 2024-08-05 12:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/lib.rs`:24 on 2024-08-05 12:52_

Going with the server crate version as `red_knot` is now a CLI crate

---

_@dhruvmanila reviewed on 2024-08-05 12:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session.rs`:69 on 2024-08-05 12:55_

> It makes me wonder if we could implement `System` on `Index` instead.

This is partly the reason the current solution exists. If we implement `System` on `Index`, we need to use an `Arc` here because it needs to exists on both the `RootDatabase` and `Session`. Otherwise we'll need to find a way to access `Index` from `RootDatabase` (not ideal). And, `Arc` would basically mean that we'd need the `MutIndexGuard`.

---

_@dhruvmanila reviewed on 2024-08-05 12:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:132 on 2024-08-05 12:56_

Oh, I see. Yeah, that could work.

---

_@dhruvmanila reviewed on 2024-08-05 13:05_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:119 on 2024-08-05 13:05_

I guess it depends on how are the permissions being utilized? I don't see any usages currently. I think one potential use-case from an editor perspective would be to re-check the file when the permission is changed but the content remains the same. Is that so?

---

_@dhruvmanila reviewed on 2024-08-05 13:06_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session/settings.rs`:34 on 2024-08-05 13:06_

(I'm assuming that this is inline with your other comment about settings.)

Why so?

---

_@MichaReiser reviewed on 2024-08-06 06:15_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:119 on 2024-08-06 06:15_

There's no immediate use case for it anymore now that we have shifted towards building a type checker. The only use we have for permissions are the permission-lints. 

---

_@dhruvmanila reviewed on 2024-08-06 08:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session/index.rs`:26 on 2024-08-06 08:40_

I've removed all settings except for the tracing related ones.

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_close.rs`:40 on 2024-08-06 08:53_

> Do we need to inform the `RootDatabase` that the file migt have changed? E.g. in the case where there are unsaved changes that get discarded?

Yeah, that's a good point. I just checked how VS Code sends requests in these cases. In either cases of the following cases, VS Code will first send in a didChange request, refresh the diagnostics and then send a close request:
* Close an edited file by having auto-save on
* Close an edited file with auto-save off and discarding the changes

I think these will be handled in the didChange notification handler in the future and shouldn't be in the didClose handler.

---

_@dhruvmanila reviewed on 2024-08-06 08:53_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-08-06 09:08_

---

_@dhruvmanila reviewed on 2024-08-06 09:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_open_notebook.rs`:45 on 2024-08-06 09:24_

Actually, it seems that the `check_file` method doesn't provide any diagnostics without the open / close logic.

---

_@dhruvmanila reviewed on 2024-08-06 10:11_

---

_Review comment by @dhruvmanila on `crates/red_knot/Cargo.toml`:3 on 2024-08-06 10:11_

https://github.com/astral-sh/ruff-vscode/pull/572

---

_@dhruvmanila reviewed on 2024-08-06 10:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_open_notebook.rs`:45 on 2024-08-06 10:15_

This is fixed in https://github.com/astral-sh/ruff/pull/12624/commits/df7ad478fecf8e5f8b1347069c690a1c4617124a

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-06 10:36_

---

_Comment by @dhruvmanila on 2024-08-06 10:36_

I'll add a basic support for non-workspace files in a follow-up PR.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_close_notebook.rs`:28 on 2024-08-06 11:05_

Should we use the same code here as in the `didClose` handler?

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_open.rs`:35 on 2024-08-06 11:05_

```suggestion
            let file = system_path_to_file(&**db, &path).unwrap();
            file.sync(db.get_mut(), &path);
```

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_open_notebook.rs`:37 on 2024-08-06 11:06_

Should we use the same code here as in `didOpen`?

---

_@MichaReiser approved on 2024-08-06 11:08_

Awesome. This looks great!

If you can find the time, it would be great to have a follow up PR to store the file on `DocumentController` rather than looking it everytime we call `check_file` (or in close etc). 

We should also add a TODO that the LSP shouldn't use any queries and instead should only use methods exposed on `RootDatabase`.

---

_Merged by @dhruvmanila on 2024-08-06 11:27_

---

_Closed by @dhruvmanila on 2024-08-06 11:27_

---

_Branch deleted on 2024-08-06 11:27_

---
