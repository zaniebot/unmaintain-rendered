```yaml
number: 13770
title: Avoid indexing the workspace for single-file mode
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/single-file-support
created_at: 2024-10-16T09:11:07Z
updated_at: 2024-10-18T05:21:44Z
url: https://github.com/astral-sh/ruff/pull/13770
synced_at: 2026-01-10T20:59:37Z
```

# Avoid indexing the workspace for single-file mode

---

_Pull request opened by @dhruvmanila on 2024-10-16 09:11_

## Summary

This PR updates the language server to avoid indexing the workspace for single-file mode.

**What's a single-file mode?**

When a user opens the file directly in an editor, and not the folder that represents the workspace, the editor usually can't determine the workspace root. This means that during initializing the server, the `workspaceFolders` field will be empty / nil.

Now, in this case, the server defaults to using the current working directory which is a reasonable default assuming that the directory would point to the one where this open file is present. This would allow the server to index the directory itself for any config file, if present.

It turns out that in VS Code the current working directory in the above scenario is the system root directory `/` and so the server will try to index the entire root directory which would take a lot of time. This is the issue as described in https://github.com/astral-sh/ruff-vscode/issues/627. To reproduce, refer https://github.com/astral-sh/ruff-vscode/issues/627#issuecomment-2401440767.

This PR updates the indexer to avoid traversing the workspace to read any config file that might be present. The first commit (https://github.com/astral-sh/ruff/pull/13770/commits/8dd2a31eef668b10b003f37fc646ca3a61f7193f) refactors the initialization and introduces two structs `Workspaces` and `Workspace`. The latter struct includes a field to determine whether it's the default workspace. The second commit (https://github.com/astral-sh/ruff/pull/13770/commits/61fc39bdb6dd0da7d594a9d2755a353eceb3a771) utilizes this field to avoid traversing.

Closes: #11366

## Editor behavior

This is to document the behavior as seen in different editors. The test scenario used has the following directory tree structure:
```
.
├── nested
│   ├── nested.py
│   └── pyproject.toml
└── test.py
```

where, the contents of the files are:

**test.py**
```py
import os
```

**nested/nested.py**
```py
import os
import math
```

**nested/pyproject.toml**
```toml
[tool.ruff.lint]
select = ["I"]
```

Steps:
1. Open `test.py` directly in the editor
2. Validate that it raises the `F401` violation
3. Open `nested/nested.py` in the same editor instance
4. This file would raise only `I001` if the `nested/pyproject.toml` was indexed

### VS Code

When (1) is done from above, the current working directory is `/` which means the server will try to index the entire system to build up the settings index. This will include the `nested/pyproject.toml` file as well. This leads to bad user experience because the user would need to wait for minutes for the server to finish indexing.

This PR avoids that by not traversing the workspace directory in single-file mode. But, in VS Code, this means that per (4), the file wouldn't raise `I001` but only raise two `F401` violations because the `nested/pyproject.toml` was never resolved.

One solution here would be to fix this in the extension itself where we would detect this scenario and pass in the workspace directory that is the one containing this open file in (1) above.

### Neovim

**tl;dr** it works as expected because the client considers the presence of certain files (depending on the server) as the root of the workspace. For Ruff, they are `pyproject.toml`, `ruff.toml`, and `.ruff.toml`. This means that the client notifies us as the user moves between single-file mode and workspace mode.

https://github.com/astral-sh/ruff/pull/13770#issuecomment-2416608055

### Helix

Same as Neovim, additional context in https://github.com/astral-sh/ruff/pull/13770#issuecomment-2417362097

### Sublime Text

**tl;dr** It works similar to VS Code except that the current working directory of the current process is different and thus the config file is never read. So, the behavior remains unchanged with this PR.

https://github.com/astral-sh/ruff/pull/13770#issuecomment-2417362097

### Zed

Zed seems to be starting a separate language server instance for each file when the editor is running in a single-file mode even though all files have been opened in a single editor instance.

(Separated the logs into sections separated by a single blank line indicating 3 different server instances that the editor started for 3 files.)

```
   0.000053375s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/projects/ruff-temp, using default settings
   0.009448792s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp
   0.009906334s DEBUG ruff:main ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/test.py
   0.011775917s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered

   0.000060583s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/projects/ruff-temp/nested, using default settings
   0.010387125s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp/nested
   0.011061875s DEBUG ruff:main ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/nested/nested.py
   0.011545208s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered

   0.000059125s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/projects/ruff-temp/nested, using default settings
   0.010857583s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp/nested
   0.011428958s DEBUG ruff:main ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/nested/other.py
   0.011893792s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
```

## Test Plan

When using the `ruff` server from this PR, we see that the server starts quickly as seen in the logs. Next, when I switch to the release binary, it starts indexing the root directory.

For more details, refer to the "Editor Behavior" section above.


---

_Label `server` added by @dhruvmanila on 2024-10-16 09:11_

---

_Marked ready for review by @dhruvmanila on 2024-10-16 09:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-10-16 09:45_

---

_Comment by @github-actions[bot] on 2024-10-16 09:49_

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

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:114 on 2024-10-16 10:48_

Nit: What interests me is why we're skipping the root itself if it is the default workspace. I think that would also make the comment easier to understand if it explains why we sometimes skip the root vs when we consider it.

---

_@MichaReiser reviewed on 2024-10-16 10:50_

The new structs are great! 

I'm not entirely sure that I understand the full extent of the change. Why are we skipping the current directory if it is the default workspace? Doesnt' that have an impact for non vs code users? Would you mind explaining in more detail how this works in today and how it affects different editors?

---

_Comment by @dhruvmanila on 2024-10-16 11:59_

> I'm not entirely sure that I understand the full extent of the change. Why are we skipping the current directory if it is the default workspace? 

We will consider any config file that's present in the current directory but not the ones that are present in any _nested_ directories.

The client didn't provide any workspace during initialization therefore we use the current working directory as the default workspace.

> Doesnt' that have an impact for non vs code users? Would you mind explaining in more detail how this works in today and how it affects different editors?

For VS Code, if the user opened only a single file in the editor then there won't be any workspace visible in the sidebar. The user can't open any files that are inside any of the nested directories. Here, the limitation would be that if there is any config file present in the directory containing the open file, that won't be considered. This is because the current working directory of the editor process is the system root directory.

For Neovim, if the user opened a single file directly via `nvim test.py`, then the client will try to find the root directory for it by looking for any of the relevant files like `pyproject.toml`, etc. For example, for `ruff` this is described at https://github.com/neovim/nvim-lspconfig/blob/541f3a2781de481bb84883889e4d9f0904250a56/lua/lspconfig/configs/ruff.lua#L3-L7. Now, if those files aren't present then the `workspaceFolders` will be empty during initialization where the server will assume the current working directory. In this case, the path would be same as that of the nvim process. The neat thing here is that if the user opened a file which is nested in the workspace and that workspace contains the root file, the Neovim client will send a request to add a new workspace folder that's pointing to the directory containing this new file. So, those settings will be considered by the server because they've been indexed.

---

_Comment by @dhruvmanila on 2024-10-16 12:03_

For Neovim,

With the following tree structure:
```
.
├── nested
│   ├── nested.py
│   └── pyproject.toml
└── test.py
```

```
   0.000071500s  INFO main ruff_server::server: No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /Users/dhruv/projects/ruff-temp
   0.000151750s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/projects/ruff-temp, using default settings
   0.004234667s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp
   0.004946542s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/test.py
   0.005483375s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
   1.204698542s  INFO     ruff:main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp/nested
   1.246196250s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/nested/nested.py
```

While, if we remove the `nested/pyproject.toml` file, then:

```
   0.000400084s  INFO main ruff_server::server: No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /Users/dhruv/projects/ruff-temp
   0.000877750s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/projects/ruff-temp, using default settings
   0.012916500s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/projects/ruff-temp
   0.014122625s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
   0.014338834s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/test.py
   1.608874959s DEBUG ruff:worker:2 ruff_server::resolve: Included path via `include`: /Users/dhruv/projects/ruff-temp/nested/nested.py
```

---

_@MichaReiser reviewed on 2024-10-16 14:17_

Okay I think I understand now what's happening but the code in `RuffSettingsIndex::new` with the early return and the `skip` doesn't make it obvious what's happening. 

I suggest that we add more extensive documentation why we don't skip the workspace directory if it is the default (because it won't take the recursive path). 

We should also add a comment to the early return for the default workspace explaining why we exit early. 

I'm still somewhat uncertain about the change itself because it means that we won't find any nested configurations for editors that don't pass a workspace root by default. The way I remember it is that VS Code is the exception and most other editors don't provide a workspace root. That would mean that this change is a breaking change for all those users, because their setup only continues to work if the ruff setting is in the workspace root (root of the cwd)

---

_Comment by @dhruvmanila on 2024-10-16 15:18_

> I'm still somewhat uncertain about the change itself because it means that we won't find any nested configurations for editors that don't pass a workspace root by default. The way I remember it is that VS Code is the exception and most other editors don't provide a workspace root. That would mean that this change is a breaking change for all those users, because their setup only continues to work if the ruff setting is in the workspace root (root of the cwd)

Neovim provides workspace root, it uses files like `pyproject.toml`, `ruff.toml`, and `.ruff.toml` to find it. I can look into what other editors are doing.

---

_Comment by @dhruvmanila on 2024-10-16 16:39_

Sublime Text using the LSP implementation as https://github.com/sublimelsp/LSP provides the workspace folders during initialization. It seems to be similar to VS Code minus the multi-root workspace support. When I open a folder in the editor, it passes it as the workspace folder but if I just open a single file, then it doesn't provide any folder. But, unlike VS Code, the `std::env::current_dir` for Sublime Text when a single file is opened returns `/Applications/Sublime Text.app/Contents/MacOS` for me:
```
   0.000099083s  INFO main ruff_server::server: No workspace(s) were provided during initialization. Using the current working directory as a default workspace...
   0.009809375s  INFO main ruff_server::session::index: Registering workspace: /Applications/Sublime Text.app/Contents/MacOS
```

The conclusion is that it wouldn't be a breaking change in Sublime Text because it already behaves correctly and the server does unnecessary work by indexing.

---

Helix also behaves in a similar manner as Neovim as it determines the workspace root based on certain set of files: https://github.com/helix-editor/helix/blob/d1b8129491124ce6068e95ccc58a7fefb1c9db45/languages.toml#L861. And, it also sends a request when a nested folder is opened with a root file in it which Helix would consider as a workspace folder (similar to Neovim):

```
2024-10-16T22:07:44.889 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"workspace/didChangeWorkspaceFolders","params":{"event":{"added":[{"name":"nested","uri":"file:///Users/dhruv/projects/ruff-temp/nested"}],"removed":[]}}}
```

---

_@MichaReiser approved on 2024-10-17 05:56_

Thanks for testing this. The case I'm concerned about is https://github.com/astral-sh/ruff/pull/10398 but what I understand from your comments is that neovim handles this correctly because it sets the cwd to the file's parent directory. 

---

_Comment by @dhruvmanila on 2024-10-17 08:44_

> Thanks for testing this. The case I'm concerned about is #10398 but what I understand from your comments is that neovim handles this correctly because it sets the cwd to the file's parent directory.

That issue seems unrelated to what's being changed here. Can you explain more on what your concern is about?

Regardless, I'll update the `RuffSettingsIndex::new` to be more clearer in what's happening using comments and possible code.

---

_@dhruvmanila reviewed on 2024-10-17 08:46_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:496 on 2024-10-17 08:46_

I've also changed this from `warn` to `info`. I don't think it's a warning i.e., not something that the user should be concerned of.

---

_Comment by @MichaReiser on 2024-10-17 09:02_

Isn't it related because it introduced the fallback to the current working directory if no workspace path was provided and this PR changes the behavior for this exact use case?

---

_Comment by @dhruvmanila on 2024-10-17 09:08_

> Isn't it related because it introduced the fallback to the current working directory if no workspace path was provided and this PR changes the behavior for this exact use case?

Yes. 

So, for Neovim specifically, it might not pass a working directory which means that the server should be running in single-file mode where we don't need to traverse the current directory (the behavior as is in this PR). If a user opened any file that's nested inside the current directory and if it contained a config file, then Neovim will make sure to inform the server about it by adding a new workspace folder using [`DidChangeWorkspaceFolders`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_didChangeWorkspaceFolders) notification.

What this means is that the server is doing extra work for single-file mode without this PR.

---

_Renamed from "Avoid indexing the workspace for single-file support" to "Avoid indexing the workspace for single-file mode" by @dhruvmanila on 2024-10-17 09:16_

---

_Comment by @dhruvmanila on 2024-10-17 14:50_

My current plan for this PR is to test it out in Zed, update the PR description with the details on different editor behavior and then merge it.

---

_Merged by @dhruvmanila on 2024-10-18 05:21_

---

_Closed by @dhruvmanila on 2024-10-18 05:21_

---

_Branch deleted on 2024-10-18 05:21_

---
