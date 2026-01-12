```yaml
number: 18939
title: "[ty] Initial support for workspace diagnostics"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/workspace-diagnostics
created_at: 2025-06-25T16:30:25Z
updated_at: 2025-07-07T13:31:17Z
url: https://github.com/astral-sh/ruff/pull/18939
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Initial support for workspace diagnostics

---

_@dhruvmanila_

## Summary

This PR adds initial support for workspace diagnostics in the ty server.

Reference spec: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_diagnostic

This is currently implemented via the **pull diagnostics method** which was added in the current version (3.17) and the server advertises it via the `diagnosticProvider.workspaceDiagnostics` server capability.

**Note:** This might be a bit confusing but a workspace diagnostics is not for a single workspace but for all the workspaces that the server handles. These are the ones that the server received during initialization. Currently, the ty server doesn't support multiple workspaces so this capability is also limited to provide diagnostics only for a single workspace (the first one if the client provided multiple).

A new `ty.diagnosticMode` server setting is added which can be either `workspace` (for workspace diagnostics) or `openFilesOnly` (for checking only open files) (default). This is same as `python.analysis.diagnosticMode` that Pyright / Pylance utilizes. In the future, we could use the value under `python.*` namespace as fallback to improve the experience on user side to avoid setting the value multiple times.

Part of: astral-sh/ty#81

## Test Plan

This capability was introduced in the current LSP version (~3 years) and the way it's implemented by various clients are a bit different. I've provided notes on what I've noticed and what would need to be done on our side to further improve the experience.

### VS Code

VS Code sends the `workspace/diagnostic` requests every ~2 second:

```
[Trace - 12:12:32 PM] Sending request 'workspace/diagnostic - (403)'.
[Trace - 12:12:32 PM] Received response 'workspace/diagnostic - (403)' in 2ms.
[Trace - 12:12:34 PM] Sending request 'workspace/diagnostic - (404)'.
[Trace - 12:12:34 PM] Received response 'workspace/diagnostic - (404)' in 2ms.
[Trace - 12:12:36 PM] Sending request 'workspace/diagnostic - (405)'.
[Trace - 12:12:36 PM] Received response 'workspace/diagnostic - (405)' in 2ms.
[Trace - 12:12:38 PM] Sending request 'workspace/diagnostic - (406)'.
[Trace - 12:12:38 PM] Received response 'workspace/diagnostic - (406)' in 3ms.
[Trace - 12:12:40 PM] Sending request 'workspace/diagnostic - (407)'.
[Trace - 12:12:40 PM] Received response 'workspace/diagnostic - (407)' in 2ms.
...
```

I couldn't really find any resource that explains this behavior. But, this does mean that we'd need to implement the caching layer via the previous result ids sooner. This will allow the server to avoid sending all the diagnostics on every request and instead just send a response stating that the diagnostics hasn't changed yet. This could possibly be achieved by using the salsa ID.

If we switch from workspace diagnostics to open-files diagnostics, the server would send the diagnostics only via the `textDocument/diagnostic` endpoint. Here, when a document containing the diagnostic is closed, the server would send a publish diagnostics notification with an empty list of diagnostics to clear the diagnostics from that document. The issue is the VS Code doesn't seem to be clearing the diagnostics in this case even though it receives the notification. (I'm going to open an issue on VS Code side for this today.)

https://github.com/user-attachments/assets/b0c0833d-386c-49f5-8a15-0ac9133e15ed

### Zed

Zed's implementation works by refreshing the workspace diagnostics whenever the content of the documents are changed. This seems like a very reasonable behavior and I was a bit surprised that VS Code didn't use this heuristic.

https://github.com/user-attachments/assets/71c7b546-7970-434a-9ba0-4fa620647f6c

### Neovim

Neovim only recently added support for workspace diagnostics (https://github.com/neovim/neovim/pull/34262, merged ~3 weeks ago) so it's only available on nightly versions.

The initial support is limited and requires fetching the workspace diagnostics manually as demonstrated in the video. It doesn't support refreshing the workspace diagnostics either, so that would need to be done manually as well. I'm assuming that these are just a temporary limitation and will be implemented before the stable release.

https://github.com/user-attachments/assets/25b4a0e5-9833-4877-88ad-279904fffaf9



---

_Label `server` added by @dhruvmanila on 2025-06-25 16:30_

---

_Label `ty` added by @dhruvmanila on 2025-06-25 16:30_

---

_Comment by @github-actions[bot] on 2025-06-25 16:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~80MB
+     memo fields = ~88MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB

cwltool (https://github.com/common-workflow-language/cwltool)
-     memo fields = ~207MB
+     memo fields = ~228MB

```
</details>


---

_@MichaReiser reviewed on 2025-06-25 16:36_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:98 on 2025-06-25 16:36_

I think I'd prefer not to introduce this new method and instead:

a) Remove the behavior that open files has today and make `check` always check all files (the LSP can call `check_file` if it only wants diagnostics for open files). The downside of this is that we may need another way to surface setting diagnostics
b) Add a `CheckMode` field

---

_Comment by @codspeed-hq[bot] on 2025-06-25 16:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fworkspace-diagnostics?runnerMode=WallTime)

### Merging #18939 will **not alter performance**

<sub>Comparing <code>dhruv/workspace-diagnostics</code> (5cc62b0) with <code>main</code> (a95c18a)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_@dhruvmanila reviewed on 2025-06-26 05:09_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/db.rs`:98 on 2025-06-26 05:09_

I actually thought of changing the current logic something like (b). I don't think we should do (a) as the playground checks only the files that are marked open. The LSP already uses `check_file` for pull diagnostic.

---

_@MichaReiser reviewed on 2025-06-26 12:35_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:98 on 2025-06-26 12:35_

I also realised that it's a bit more complicated because of virtual files... we can never discover them by walking the file try. That means we'll need to check the open files when calling `check` or we'll miss those files. 

---

_Review comment by @dhruvmanila on `crates/ty_project/src/db.rs`:98 on 2025-06-27 04:04_

Oh good point. I think that'll require https://github.com/astral-sh/ty/issues/619 because currently the server uses the `check_file` method directly but to include the virtual files in the workspace diagnostics, we need to know the set of virtual files that are available. After #619, we can get the `open_files` set and filter based on virtual path.

---

_@dhruvmanila reviewed on 2025-06-27 04:04_

---

_@dhruvmanila reviewed on 2025-06-27 04:09_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/db.rs`:98 on 2025-06-27 04:09_

Or, we could just ask the server index to get all the open virtual files.

---

_@MichaReiser reviewed on 2025-06-27 06:20_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:98 on 2025-06-27 06:20_

> Or, we could just ask the server index to get all the open virtual files.

I think that would work for workspace diagnostics but not necessarily how we use `open_files` in `check_file_impl` to determine if the file is open. 

---

_Renamed from "[ty] [WIP] Support workspace diagnostics" to "[ty] Initial support for workspace diagnostics" by @dhruvmanila on 2025-06-27 11:35_

---

_Comment by @github-actions[bot] on 2025-06-27 11:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2025-06-27 14:19_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:98 on 2025-06-27 14:19_

Something else we would have to be careful about when moving it to `System` is that we can't use it in any salsa query because the result can change from the outside.

---

_Marked ready for review by @dhruvmanila on 2025-06-30 06:44_

---

_Review requested from @carljm by @dhruvmanila on 2025-06-30 06:44_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-30 06:44_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-30 06:44_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-30 06:44_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-30 06:57_

---

_Review request for @carljm removed by @dhruvmanila on 2025-06-30 13:49_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-06-30 13:49_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-06-30 13:49_

---

_Review requested from @BurntSushi by @dhruvmanila on 2025-06-30 13:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-06-30 13:49_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/notifications/did_close.rs`:48 on 2025-07-02 16:40_

How come this doesn't need to be done when in workspace diagnostic mode?

---

_@BurntSushi approved on 2025-07-02 16:42_

Nice! This looks sensible to me.

---

_@dhruvmanila reviewed on 2025-07-03 05:39_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_close.rs`:48 on 2025-07-03 05:39_

Because in workspace mode all diagnostics should be visible regardless of whether a document is open or close in an editor. So, we shouldn't clear the diagnostics when a user closes a file in the editor.

---

_Merged by @dhruvmanila on 2025-07-03 11:04_

---

_Closed by @dhruvmanila on 2025-07-03 11:04_

---

_Branch deleted on 2025-07-03 11:04_

---

_@BurntSushi reviewed on 2025-07-03 11:40_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/notifications/did_close.rs`:48 on 2025-07-03 11:40_

Ahhhh makes sense of course.

---

_Comment by @MichaReiser on 2025-07-07 13:31_

This is great. We may want to iterate on how we handle the different check modes in `ty_project` but what you have here is a good solution to start from.

---
