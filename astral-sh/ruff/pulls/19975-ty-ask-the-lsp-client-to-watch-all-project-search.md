```yaml
number: 19975
title: "[ty] Ask the LSP client to watch all project search paths"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/watch-search-paths
created_at: 2025-08-18T18:32:09Z
updated_at: 2025-08-19T14:57:09Z
url: https://github.com/astral-sh/ruff/pull/19975
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Ask the LSP client to watch all project search paths

---

_@BurntSushi_

This change rejiggers how we register globs for file watching with the
LSP client. Previously, we registered a few globs like `**/*.py`,
`**/pyproject.toml` and more. There were two problems with this
approach.

Firstly, it only watches files within the project root. Search paths may
be outside the project root. Such as virtualenv directory.

Secondly, there is variation on how tools interact with virtual
environments. In the case of uv, depending on its link mode, we might
not get any file change notifications after running `uv add foo` or
`uv remove foo`.

To remedy this, we instead just list for file change notifications on
all files for all search paths. This simplifies the globs we use, but
does potentially increase the number of notifications we'll get.
However, given the somewhat simplistic interface supported by the LSP
protocol, I think this is unavoidable (unless we used our own file
watcher, which has its own considerably downsides). Moreover, this is
seemingly consistent with how `ty check --watch` works.

This also required moving file watcher registration to *after*
workspaces are initialized, or else we don't know what the right search
paths are.

This change is in service of #19883, which in order for cache
invalidation to work right, the LSP client needs to send notifications
whenever a dependency is added or removed. This change should make that
possible.

I tried this patch with #19883 in addition to my work to activate Salsa
caching, and everything seems to work as I'd expect. That is,
completions no longer show stale results after a dependency is added or
removed.


---

_Review requested from @carljm by @BurntSushi on 2025-08-18 18:32_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-08-18 18:32_

---

_Review requested from @sharkdp by @BurntSushi on 2025-08-18 18:32_

---

_Review requested from @dcreager by @BurntSushi on 2025-08-18 18:32_

---

_Review request for @dcreager removed by @BurntSushi on 2025-08-18 18:32_

---

_Review request for @carljm removed by @BurntSushi on 2025-08-18 18:32_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-08-18 18:32_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-08-18 18:32_

---

_Comment by @github-actions[bot] on 2025-08-18 18:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-18 18:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-08-18 20:07_

---

_Label `ty` added by @AlexWaygood on 2025-08-18 20:07_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:162 on 2025-08-19 04:47_

I think this needs to be moved inside `self.session.initialize_workspace` because we call that method directly (instead of taking this branch) for clients that doesn't support `workspace/configuration`. Refer to the other reference call of `self.session.initialize_workspace`.

The `try_register_file_watcher` should also be moved on the `Session` then. At that point, I think it would be useful to use the `Session::register_dynamic_capabilities` method to include the logic from `try_register_file_watcher` by checking whether the client supports it and then adding the `Unregistration` and `Registration` accordingly to the vector.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:296 on 2025-08-19 04:54_

I think it would be useful to move this on `Session` and re-use the `register_dynamic_capabilities` method. Refer to my other comment.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:299 on 2025-08-19 04:56_

nit: this seems to only be used once, should we inline the logic unless I've missed any other usage?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:336 on 2025-08-19 04:57_

What's the reason that this message is at the warning level? I think it should be fine at the info level because I don't think the user is in control of this and they cannot act on this warning.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:366 on 2025-08-19 04:59_

Yeah, I forgot that we didn't unregister the file watcher before registering it. I think we can re-use the `Session::register_dynamic_capabilities`. The spec doesn't mention whether the server can unregister a capability which wasn't registered before but I've avoided that in the above mentioned method. I think we should follow that.

---

_@dhruvmanila approved on 2025-08-19 05:02_

This looks great!

The only thing that would be required in this PR is to move the `try_register_file_watcher` to be called inside `initialize_workspaces` and optionally re-use the `register_dynamic_capabilities` for this.

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:326 on 2025-08-19 05:58_

Can you expand on what issues you ran into and what you used as base uri so that a future reader knows what to test if they want to fix file watching for clients not supporting relative paths (I'm not suggesting we should fix this now, I just wouldn't know what I'd have to test)

---

_@MichaReiser approved on 2025-08-19 06:04_

Nice, thank you for fixing this

---

_@BurntSushi reviewed on 2025-08-19 11:43_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:162 on 2025-08-19 11:43_

Got it. That makes sense!

---

_@BurntSushi reviewed on 2025-08-19 11:47_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:336 on 2025-08-19 11:47_

Just copying what was already there. Specifically:

https://github.com/astral-sh/ruff/blob/e5c091b850a95601c86d7db718953a91de1e1b48/crates/ty_server/src/server/main_loop.rs#L290-L293

Should I change that to an info-level message as well?

---

_@BurntSushi reviewed on 2025-08-19 11:49_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:299 on 2025-08-19 11:49_

No, you're right, it's only used once. But I'd prefer to keep it. `make_relative_watcher` is only used once too. IMO it makes the code a touch easier to read as it pulls out some mildly tedious construction of LSP glob patterns and lets the reader focus on the surrounding logic. The "legacy" branch becomes straight-forwardly simple and the relative glob branch lets you focus on how the search paths are collected and deduplicated.

---

_@MichaReiser reviewed on 2025-08-19 11:57_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:336 on 2025-08-19 11:57_

My reasoning for using warn for `"Client does not support file system watching"` was that it can lead to a degraded experience. In hindsight, I think it would have been great to phrase the message like: `Your LSP client doesn't support file watching: You may see stale results when files change outside the editor`. 

I'd suggest that we keep the message at warning severity if it impacts the user experience and include a summary of what behavior they have to expect

---

_@BurntSushi reviewed on 2025-08-19 12:11_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:336 on 2025-08-19 12:11_

Sounds good. Done.

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:326 on 2025-08-19 12:22_

I expanded on this a bit:

```rust
        // We also want to watch everything in the search paths as
        // well. But this seems to require "relative" watcher support.
        // I had trouble getting this working without using a base uri.
        //
        // Specifically, I tried this for each search path:
        //
        //     make_watcher(&format!("{path}/**"))
        //
        // But while this seemed to work for the project root, it
        // simply wouldn't result in any file notifications for changes
        // to files outside of the project root.
```

---

_@BurntSushi reviewed on 2025-08-19 12:22_

---

_@BurntSushi reviewed on 2025-08-19 14:43_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/main_loop.rs`:162 on 2025-08-19 14:43_

This is done. Much cleaner!

---

_Merged by @BurntSushi on 2025-08-19 14:57_

---

_Closed by @BurntSushi on 2025-08-19 14:57_

---

_Branch deleted on 2025-08-19 14:57_

---
