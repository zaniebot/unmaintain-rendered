```yaml
number: 11140
title: "`ruff server`: Support a custom TOML configuration file"
type: pull_request
state: merged
author: snowsignal
labels:
  - configuration
  - server
assignees: []
merged: true
base: main
head: jane/server/settings/custom-configuration-toml
created_at: 2024-04-25T02:35:58Z
updated_at: 2024-04-26T23:51:49Z
url: https://github.com/astral-sh/ruff/pull/11140
synced_at: 2026-01-10T22:37:01Z
```

# `ruff server`: Support a custom TOML configuration file

---

_Pull request opened by @snowsignal on 2024-04-25 02:35_

## Summary

Closes #10985.

The server now supports a custom TOML configuration file as a client setting. The setting must be an absolute path to a file. If the file is called `pyproject.toml`, the server will attempt to parse it as a pyproject file - otherwise, it will attempt to parse it as a `ruff.toml` file, even if the file has a name besides `ruff.toml`.

If an option is set in both the custom TOML configuration file and in the client settings directly, the latter will be used.

## Test Plan

1. Create a `ruff.toml` file outside of the workspace you are testing. Set an option that is different from the one in the configuration for your test workspace.
2. Set the path to the configuration in NeoVim:
```lua
require('lspconfig').ruff.setup {
    init_options = {
      settings = {
        configuration = "absolute/path/to/your/configuration"
      }
    }
}
```
3. Confirm that the option in the configuration file is used, regardless of what the option is set to in the workspace configuration.
4. Add the same option, with a different value, to the NeoVim configuration directly. For example:
```lua
require('lspconfig').ruff.setup {
    init_options = {
      settings = {
        configuration = "absolute/path/to/your/configuration",
        lint = {
          select = []
        }
      }
    }
}
```
5. Confirm that the option set in client settings is used, regardless of the value in either the custom configuration file or in the workspace configuration.

---

_Label `configuration` added by @snowsignal on 2024-04-25 02:35_

---

_Label `server` added by @snowsignal on 2024-04-25 02:35_

---

_Renamed from "`ruff server` supports a custom TOML configuration file" to "`ruff server`: support a custom TOML configuration file" by @snowsignal on 2024-04-25 02:36_

---

_Renamed from "`ruff server`: support a custom TOML configuration file" to "`ruff server`: Support a custom TOML configuration file" by @snowsignal on 2024-04-25 02:36_

---

_@snowsignal reviewed on 2024-04-25 02:38_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:34 on 2024-04-25 02:38_

I'm open to using a better name for this 😅 

---

_@charliermarsh reviewed on 2024-04-25 02:50_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:162 on 2024-04-25 02:50_

If `configuration` is specified, should we ignore project configuration entirely? (My read is that we're merging them.) That's how `--config` works in the Ruff CLI.

---

_@charliermarsh reviewed on 2024-04-25 02:51_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:34 on 2024-04-25 02:51_

I think we'll need to expand tildes and environment variables for settings that users provide in VS Code. (We do this within Ruff when you pass `--config`.) Should that happen within the LSP?

---

_Comment by @github-actions[bot] on 2024-04-25 02:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@snowsignal reviewed on 2024-04-25 03:16_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:162 on 2024-04-25 03:16_

My plan was to merge them - if we wanted to allow for this behavior, we could introduce an additional resolution strategy that ignores project configuration entirely.

---

_@charliermarsh reviewed on 2024-04-25 03:18_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:162 on 2024-04-25 03:18_

I worry about adding a bunch of resolution strategies. They're hard to reason about -- even I'm having trouble keeping the rules straight.

I think merging here is surprising given that the Ruff CLI does _not_ do this -- if you provide a configuration file, that's the configuration you get.

---

_@MichaReiser reviewed on 2024-04-25 07:07_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:34 on 2024-04-25 07:07_

I would expect this to be a VS Code extension functionality, except if we support it for `ruff check --config` too. Except if we think that's something that's also useful for neovim (does neovim support expanding environment variables). 

Relevant issue https://github.com/astral-sh/ruff-vscode/issues/448

---

_Comment by @GaetanLepage on 2024-04-25 13:42_

In the absence of a project configuration for ruff, will the LS load the seetings from `~/.config/ruff/ruff.toml` like the CLI does ?

---

_@charliermarsh reviewed on 2024-04-26 00:22_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:162 on 2024-04-26 00:22_

What's your current thinking here @snowsignal?

---

_Comment by @charliermarsh on 2024-04-26 00:22_

It should yes -- not sure if it does yet though @snowsignal?

---

_Comment by @snowsignal on 2024-04-26 07:55_

@GaetanLepage @charliermarsh I actually don't think we do this, yet - I've [filed an issue](https://github.com/astral-sh/ruff/issues/11158) to support this.

---

_@snowsignal reviewed on 2024-04-26 07:56_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:162 on 2024-04-26 07:56_

I've been deliberating over this, and I actually do think it makes sense to have this reflect the behavior of `--config`. Having three different groups of options merging into each other is a lot of cognitive complexity, and a full override does feel like a more 'intuitive' option.

My assumption is that if we were using a `prioritizeWorkspace` resolution strategy, this configuration file would be ignored.

---

_@snowsignal reviewed on 2024-04-26 22:22_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:34 on 2024-04-26 22:22_

Environment variable resolution is definitely something we should have, but I think that's beyond the scope of this PR. As for tilde resolution - shouldn't that be resolved already in `parse_pyproject_toml`?

---

_Review requested from @charliermarsh by @snowsignal on 2024-04-26 22:22_

---

_@charliermarsh reviewed on 2024-04-26 22:37_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 22:37_

Haha yeah, three sources of configuration is kind of a lot. But maybe we can reframe this as two sources, since the `config_from_file` is actually just part of the editor configuration, right...?

---

_@charliermarsh reviewed on 2024-04-26 22:37_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 22:37_

How about this:
```diff
diff --git a/crates/ruff_server/src/session/workspace/ruff_settings.rs b/crates/ruff_server/src/session/workspace/ruff_settings.rs
index ab8a8447e..766b2aa61 100644
--- a/crates/ruff_server/src/session/workspace/ruff_settings.rs
+++ b/crates/ruff_server/src/session/workspace/ruff_settings.rs
@@ -151,18 +151,18 @@ impl<'a> ConfigurationTransformer for EditorConfigurationTransformer<'a> {
             ..Default::default()
         };
 
-        if let Some(config_file_path) = configuration {
+        // Merge in the editor-specified configuration file, if any.
+        let editor_configuration = if let Some(config_file_path) = configuration {
             match open_configuration_file(&config_file_path, project_root) {
-                // if a custom configuration file was given and the configuration preference is
-                // _not_ filesystem-first, return early with the combined editor configuration.
-                Ok(config_from_file) => {
-                    if configuration_preference != ConfigurationPreference::FilesystemFirst {
-                        return editor_configuration.combine(config_from_file);
-                    }
+                Ok(config_from_file) => editor_configuration.combine(config_from_file),
+                Err(err) => {
+                    tracing::error!("Unable to find custom configuration file {err}");
+                    editor_configuration
                 }
-                Err(err) => tracing::error!("Unable to find custom configuration file {err}"),
             }
-        }
+        } else {
+            editor_configuration
+        };
 
         match configuration_preference {
             ConfigurationPreference::EditorFirst => {
```

---

_@charliermarsh reviewed on 2024-04-26 22:38_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 22:38_

The basic idea being: the editor configuration is always `editor_configuration.combine(config_from_file)` (if it exists); then, we just follow the match below.

---

_@charliermarsh approved on 2024-04-26 22:38_

I have one suggestion to simplify the implementation and semantics.

---

_@snowsignal reviewed on 2024-04-26 22:41_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:34 on 2024-04-26 22:41_

Okay, I did some testing and it looks like tilde expansion isn't handled by the standard library automatically. I'll open a follow-up PR that uses `shellexpand` to expand tildes and environment variables.

---

_@snowsignal reviewed on 2024-04-26 22:48_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 22:48_

Wouldn't this mean that the custom configuration file would get merged with filesystem configuration, though? I thought we wanted to ignore filesystem configuration similar to `--config`.

That being said, we already have another way to ignore filesystem configuration (with the `EditorFirst` configuration preference), so maybe those behaviors should be detached and it's up to the user. But it might not be the most intuitive experience.

---

_@charliermarsh reviewed on 2024-04-26 23:14_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 23:14_

I thought `EditorOnly` was the solution to this, though?

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 23:15_

Sorry, I meant to say `EditorOnly` instead of `EditorFirst`. I'm fine with that as a solution, I just wanted to make sure we were on the same page 👍 

---

_@snowsignal reviewed on 2024-04-26 23:15_

---

_@charliermarsh reviewed on 2024-04-26 23:21_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:161 on 2024-04-26 23:21_

I think it's ok.

---

_Merged by @snowsignal on 2024-04-26 23:46_

---

_Closed by @snowsignal on 2024-04-26 23:46_

---

_Branch deleted on 2024-04-26 23:46_

---
