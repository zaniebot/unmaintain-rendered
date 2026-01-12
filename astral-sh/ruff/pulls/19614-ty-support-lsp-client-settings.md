```yaml
number: 19614
title: "[ty] Support LSP client settings"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: dhruv/client-settings
created_at: 2025-07-29T09:13:37Z
updated_at: 2025-08-06T13:07:23Z
url: https://github.com/astral-sh/ruff/pull/19614
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Support LSP client settings

---

_@dhruvmanila_

## Summary

This PR implements support for providing LSP client settings.

The complementary PR in the ty VS Code extension: astral-sh/ty-vscode#106.

Notes for the previous iteration of this PR is in https://github.com/astral-sh/ruff/pull/19614#issuecomment-3136477864 (click on "Details").

Specifically, this PR splits the client settings into 3 distinct groups. Keep in mind that these groups are not visible to the user, they're merely an implementation detail. The groups are:
1. `GlobalOptions` - these are the options that are global to the language server and will be the same for all the workspaces that are handled by the server
2. `WorkspaceOptions` - these are the options that are specific to a workspace and will be applied only when running any logic for that workspace
3. `InitializationOptions` - these are the options that can be specified during initialization

The initialization options are a superset that contains both the global and workspace options flattened into a 1-dimensional structure. This means that the user can specify any and all fields present in `GlobalOptions` and `WorkspaceOptions` in the initialization options in addition to the fields that are _specific_ to initialization options.

From the current set of available settings, following are only available during initialization because they are required at that time, are static during the runtime of the server and changing their values require a restart to take effect:
- `logLevel`
- `logFile`

And, following are available under `GlobalOptions`:
- `diagnosticMode`

And, following under `WorkspaceOptions`:
- `disableLanguageServices`
- `pythonExtension` (Python environment information that is populated by the ty VS Code extension)

### `workspace/configuration`

This request allows server to ask the client for configuration to a specific workspace. But, this is only supported by the client that has the `workspace.configuration` client capability set to `true`. What to do for clients that don't support pulling configurations?

In that case, the settings needs to be provided in the initialization options and updating the values of those settings can only be done by restarting the server. With the way this is implemented, this means that if the client does not support pulling workspace configuration then there's no way to specify settings specific to a workspace. Earlier, this would've been possible by providing an array of client options with an additional field which specifies which workspace the options belong to but that adds complexity and clients that actually do not support `workspace/configuration` would usually not support multiple workspaces either.

Now, for the clients that do support this, the server will initiate the request to get the configuration for all the workspaces at the start of the server. Once the server receives these options, it will resolve them for each workspace as follows:
1. Combine the client options sent during initialization with the options specific to the workspace creating the final client options that's specific to this workspace
2. Create a global options by combining the global options from (1) for all workspaces which in turn will also combine the global options sent during initialization

The global options are resolved into the global settings and are available on the `Session` which is initialized with the default global settings. The workspace options are resolved into the workspace settings and are available on the respective `Workspace`.

The `SessionSnapshot` contains the global settings while the document snapshot contains the workspace settings. We could add the global settings to the document snapshot but that's currently not needed.

### Document diagnostic dynamic registration

Currently, the document diagnostic server capability is created based on the `diagnosticMode` sent during initialization. But, that wouldn't provide us with the complete picture. This means the server needs to defer registering the document diagnostic capability at a later point once the settings have been resolved.

This is done using dynamic registration for clients that support it. For clients that do not support dynamic registration for document diagnostic capability, the server advertises itself as always supporting workspace diagnostics and work done progress token.

This dynamic registration now allows us to change the server capability for workspace diagnostics based on the resolved `diagnosticMode` value. In the future, once `workspace/didChangeConfiguration` is supported, we can avoid the server restart when users have changed any client settings.

## Test Plan

Add integration tests and recorded videos on the user experience in various editors:

### VS Code

For VS Code users, the settings experience is unchanged because the extension defines it's own interface on how the user can specify the server setting. This means everything is under the `ty.*` namespace as usual.

https://github.com/user-attachments/assets/c2e5ba5c-7617-406e-a09d-e397ce9c3b93

### Zed

For Zed, the settings experience has changed. Users can specify settings during initialization:

```json
{
  "lsp": {
    "ty": {
      "initialization_options": {
        "logLevel": "debug",
        "logFile": "~/.cache/ty.log",
        "diagnosticMode": "workspace",
        "disableLanguageServices": true
      }
    },
  }
}
```

Or, can specify the options under the `settings` key:

```json
{
  "lsp": {
    "ty": {
      "settings": {
        "ty": {
          "diagnosticMode": "openFilesOnly",
          "disableLanguageServices": true
        }
      },
      "initialization_options": {
        "logLevel": "debug",
        "logFile": "~/.cache/ty.log"
      }
    },
  }
}
```

The `logLevel` and `logFile` setting still needs to go under the initialization options because they're required by the server during initialization.

We can remove the nesting of the settings under the "ty" namespace by updating the return type of https://github.com/zed-extensions/ty/blob/db9ea0cdfd7352529748ef5bf729a152c0219805/src/tychecker.rs#L45-L49 to be wrapped inside `ty` directly so that users can avoid doing the double nesting.

There's one issue here which is that if the `diagnosticMode` is specified in both the initialization option and settings key, then the resolution is a bit different - if either of them is set to be `workspace`, then it wins which means that in the following configuration, the diagnostic mode is `workspace`:

```json
{
  "lsp": {
    "ty": {
      "settings": {
        "ty": {
          "diagnosticMode": "openFilesOnly"
        }
      },
      "initialization_options": {
        "diagnosticMode": "workspace"
      }
    },
  }
}
```

This behavior is mainly a result of combining global options from various workspace configuration results. Users should not be able to provide global options in multiple workspaces but that restriction cannot be done on the server side. The ty VS Code extension restricts these global settings to only be set in the user settings and not in workspace settings but we do not control extensions in other editors.

https://github.com/user-attachments/assets/8e2d6c09-18e6-49e5-ab78-6cf942fe1255

### Neovim

Same as in Zed.

### Other

Other editors that do not support `workspace/configuration`, the users would need to provide the server settings during initialization.



---

_Label `server` added by @dhruvmanila on 2025-07-29 09:13_

---

_Label `ty` added by @dhruvmanila on 2025-07-29 09:13_

---

_Comment by @github-actions[bot] on 2025-07-29 09:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-29 09:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/goto_definition.rs`:34 on 2025-07-30 08:10_

I like the new terminology used here

---

_@MichaReiser reviewed on 2025-07-30 08:10_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:106 on 2025-07-30 08:11_

What would you think of naming `global_settings`, `window_settings` (I'm okay with global settings. I'm just wondering if it's worth aligning with VS code's terminology)

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/settings.rs`:21 on 2025-07-30 08:18_

I do find `combine` on the `Settings` struct a bit surprsing. The way this works in ty (and uv and ruff) is that you can combine `Options`. All options are then resolved to a single `Settings`. 


The way this could look like in your PR is that you have a `GlobalOptions` struct. Each workspace configuration contains a `GlobalOptions` field and the server can combine all of these together. `GlobalOptions` then implements an `into_settings` which resolves any default values. 

The benefit of the `Options` struct is that `diagnostic_mode` can be an `Option<DiagnosticMode>`. This allows us to distinguish between: *initialize didn't set `DiagnosticMode`* and *the user specified the default `DiagnsoticMode`*.



---

_Review comment by @MichaReiser on `crates/ty_server/src/capabilities.rs`:252 on 2025-07-30 08:19_

This feels a bit out of place. The function doesn't contain any code related to capabilities

---

_Comment by @MichaReiser on 2025-07-30 08:23_

I like where this is going. It clarifies a lot of the differences!

> One way to improve would be to accept all the settings in initialization options and consider them global settings. Then, if the client supports workspace/configuration request, we can ask the client for workspace specific configuration that would take precedence over the global settings.

This makes a lot of sense to me and seems more intuitive. Especially given that most editors don't support workspaces.

> Here, there are two kinds of settings that options.into_settings will return - GlobalSettings are the ones that are applied globally e.g., ty.diagnosticMode and WorkspaceSettings are the ones that are applied on a per-workspace level e.g., ty.disableLanguageServices.

I don't fully understand this part. I also left an inline comment related to this but I think the structure we want to deserialize into in the workspace configuration handler is one that has two fields: `global_options` and `workspace_options` (we can use `serde(flatten)` to hide this detail from clients). I think this will help clarify which fields are workspace specific vs which fields are global. It also allows you to implement `combine` on `GlobalOptions` and not on settings (following the pattern we use in the CLI)



---

_@MichaReiser reviewed on 2025-07-30 08:23_

---

_Comment by @dhruvmanila on 2025-07-30 13:58_

Notes for the previous iteration:

<details><summary>Details</summary>
<p>

This PR adds support for client settings via the `workspace/configuration` request.

Currently, the settings can only be passed in via the initialization options which are limited because any change to the settings require a server restart to take effect. This is why Micha added the infrastructure to request configuration for a specific workspace via the `workspace/configuration` request.

This PR implements the point 1, 2, 3 from https://github.com/astral-sh/ty/issues/82#issuecomment-3026608030 i.e.,
- Create a `InitializationOptions` that only contains `logLevel` and `logFiles`. These are the static options that actually require a server restart to take effect
- Start the server initialization process where the server initiates a request to get all workspace configurations via `workspace/configuration` request endpoint and defers all notifications and requests until the initialization is complete
- Once the server receives the response from `workspace/configuration` request, it initializes the workspace options with the received data
- Here, there are two kinds of settings that `options.into_settings` will return - `GlobalSettings` are the ones that are applied globally e.g., `ty.diagnosticMode` and `WorkspaceSettings` are the ones that are applied on a per-workspace level e.g., `ty.disableLanguageServices`. Now, the protocol only allows to request configuration specific to a workspace, there's no concept of global settings that are dynamic in the spec, so we model that internally using `GlobalSettings` but this requires taking all the `GlobalSettings` from all the configured workspaces and combining them. This could result in a mis-match where one workspace has "workspace" diagnostic mode while other has "openFilesOnly" diagnostic mode. In this case, the server should show this mismatch somehow but currently it doesn't.

This split is recommended in https://github.com/microsoft/language-server-protocol/issues/567 and is also followed by Dart where [initialization options](https://github.com/dart-lang/sdk/blob/main/pkg/analysis_server/tool/lsp_spec/README.md#initialization-options) are separate from [workspace options](https://github.com/dart-lang/sdk/blob/main/pkg/analysis_server/tool/lsp_spec/README.md#client-workspace-configuration).

But, there seems to be one issue which is that the way `workspace/configuration` works is that the client can store these settings in any structure it wants which means that the way the user would need to store the settings might differ from editor to editor. I've provided examples on what it would look like with this PR.

For VS Code:
```json
{
	"ty.logLevel": "debug",
	"ty.diagnosticMode": "workspace"
}
```

For Neovim:
```lua
{
	settings = {
		diagnosticMode = "workspace",
	},
	init_options = {
		logLevel = "debug",
	}
}
```

For Zed:
```json
{
  "lsp": {
    "ty": {
      "settings": {
        "ty": {
          "diagnosticMode": "openFilesOnly"
        }
      },
	  "initialization_options": {
		"logLevel": "debug"
	  }
    }
  }
}
```

For Helix:
```toml
[language-server.ty.config]
logLevel = "debug"
ty = { diagnosticMode = "workspace" }
```

### Question

Based on all of the changes that I've made so far and what I'm observing when using this PR to define the settings in an editor is that this model still seems a bit confusing from a user perspective. One way to improve would be to accept all the settings in initialization options and consider them global settings. Then, if the client supports `workspace/configuration` request, we can ask the client for workspace specific configuration that would take precedence over the global settings. But, note that some settings like `diagnosticMode` are still global so a workspace setting cannot override the value of `diagnosticMode`. We could show a warning if the user has done this.

</p>
</details> 

---

_@dhruvmanila reviewed on 2025-07-30 14:23_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:106 on 2025-07-30 14:23_

I'm not a big fan of `window_settings` as that seems too specific to VS Code (there are no windows for Neovim :)). I'll keep this as global settings.

---

_@dhruvmanila reviewed on 2025-07-30 14:23_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/settings.rs`:21 on 2025-07-30 14:23_

Thank you, I've implemented this change.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/capabilities.rs`:252 on 2025-07-30 14:24_

It does, the `DiagnosticOptions` are the server capability for document diagnostic.

---

_@dhruvmanila reviewed on 2025-07-30 14:24_

---

_Marked ready for review by @dhruvmanila on 2025-08-04 12:18_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-04 12:18_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-04 12:18_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-04 12:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-05 02:09_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_symbols.rs`:33 on 2025-08-05 04:18_

We can't query this setting because it's scope is limited to a specific workspace and these endpoints are for _all_ the workspaces managed by the server i.e., this setting is present under `WorkspaceSettings` but the `SessionSnapshot` does not know which workspace is this endpoint for.

I think this is fine because all the workspace related endpoints except for workspace diagnostics are triggered by user explicitly. But, if we do want to enforce this, we could use the settings value from each workspace and act on it accordingly. For example, in workspace symbols, we would skip the workspace if language services are disabled for that workspace:

```rs
        // Iterate through all projects in the session
        for db in snapshot.projects() {
			// Find the workspace which this project belongs to
			// Check if language services are enabled or not
			if language_services_disabled {
				continue;
			}
```

---

_@dhruvmanila reviewed on 2025-08-05 04:18_

---

_Review request for @sharkdp removed by @sharkdp on 2025-08-05 06:55_

---

_Label `great writeup` added by @MichaReiser on 2025-08-05 08:33_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_close.rs`:62 on 2025-08-05 08:38_

I think this now justifies calling `session.bump_revision` in `close_file`. Overall, I think it's better to all `bump_file` one time too many than too few times

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:222 on 2025-08-05 08:40_

Should we call `self.session.initialize_workspaces(workspaces_with_options, &client);` instead of queing an action?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:37 on 2025-08-05 08:41_

Can we use the `resolved_client_capabilities` from the `session` instead of storing it both on session and server?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:249 on 2025-08-05 08:43_

Let's add a custom message here in case there is a faulty client (or a client where a user needs to specify this manually). 

A custom message at least gives users (and us) a hint what's going wrong. This is especially useful in release builds where we strip debug information.

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:266 on 2025-08-05 08:44_

I know it's unrelated to your changes, so I'm okay deferring this. But should we maybe show a pop up if this happens? E.g. could this happen if a user configures zed or neovim incorrectly?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/main_loop.rs`:297 on 2025-08-05 08:44_

Nice!

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:31 on 2025-08-05 08:45_

These docs are great! 

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:57 on 2025-08-05 08:47_

Nit: `Result<InitializationOptions, (InitializationOptions, serde_json::Eror)>` might be more explicit (and Rust will warn if the caller doesn't handle the error). I think we could even return `Result<InitializationOptions, serde_json::Error>` and leave it to the caller to fall back to the default options

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:105 on 2025-08-05 08:49_

I think you can derive this `Combnine` implementation, given that `GlobalOptions` and `workspaceOptions` implement `Combine`.

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:109 on 2025-08-05 08:51_

Maybe document this type too. I was a bit surprised that `log_level` isn't part of `GlobalOptions` but I think the reason is that `GlobalOptions` stores all non-static options.

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:127 on 2025-08-05 08:51_

You can derive `Combine`

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:198 on 2025-08-05 08:52_

You can derive `Combine`

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:229 on 2025-08-05 08:54_

Can we add a comment why this is necessary

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:243 on 2025-08-05 08:58_

I don't think this is a correct use of `unreachable` and we should use `panic` instead. Non VS code clients could set `ty.pythonExtension` in the initialization and workspace options and it would deserialize just fine. I'm also not sure if it's worth panicing or if we should just always take the first value that is `Some`. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:332 on 2025-08-05 08:59_

Hmm. I'd rather not have a third `Combine` trait. Can't we use the `Combine` trait from `ty_project` directly?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:58 on 2025-08-05 09:01_

Nit: This is a pre-existing issue but let's maybe use `context` to add some extra information what's failing to deserialize
```suggestion
        } = serde_json::from_value(init_value).context("Failed to deserialize the `initializationParams`)?;
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:70 on 2025-08-05 09:01_

I'd change this to debug or use a proper `Display` for `initialization_options`. `info` level messages are meant for users. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:77 on 2025-08-05 09:03_

I'm inclined to move the "fallback to default" logic here or we risk that we someday return something other than the defaults and forget to adjust the log message.

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:462 on 2025-08-05 09:05_

Can't you call `combine` directly on `combined_global_options` (`Combine` is implemented for `Option<T>` where `T: Combine`)

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:542 on 2025-08-05 09:06_

Nit: We could move the `!= CheckMode::AllFiles` into `set_check_mode` method (so that it only sets the new mode if it is different to avoid triggering a new salsa revision)

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/snapshots/e2e__initialize__open_files_diagnostic_registration_via_initialization.snap`:1 on 2025-08-05 09:09_

Should we use inline snapshots for those. They're very small

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:229 on 2025-08-05 09:11_

Maybe use `expect` or `or_else` to give the test author more context about what's going wrong.

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:196 on 2025-08-05 09:12_

Isn't it required that we have one option for each `url`? What's the reason for allowing `None` for the configuration?

---

_@MichaReiser approved on 2025-08-05 09:15_

This is awesome. Thank you.

The only change I'd like to see before landing this PR is to deduplicate `Combine`. We already have two implementations of it (one in ty and one in ruff). I'd rather avoid a third.

---

_@dhruvmanila reviewed on 2025-08-05 10:29_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_close.rs`:62 on 2025-08-05 10:29_

Yeah, I think that's correct and I think it should also be bumped in the virtual file branch. So, closing the virtual file or a file that does not belong to a workspace should refresh the workspace diagnostics because the diagnostics that are from that file should not be present now.

---

_@dhruvmanila reviewed on 2025-08-05 10:31_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_close.rs`:62 on 2025-08-05 10:31_

Oh, `bump_revision` is private to `Session`.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:222 on 2025-08-05 10:37_

Ah yeah, I think that's possible here. Thanks

---

_@dhruvmanila reviewed on 2025-08-05 10:37_

---

_@dhruvmanila reviewed on 2025-08-05 10:40_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:37 on 2025-08-05 10:40_

Yes! I've updated it.

---

_@dhruvmanila reviewed on 2025-08-05 10:43_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:266 on 2025-08-05 10:43_

Yeah, I think that could happen and it might go unnoticed, I think it'd be useful to add a popup.

---

_@dhruvmanila reviewed on 2025-08-05 10:46_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:266 on 2025-08-05 10:46_

I think it'd be better as a follow-up instead

---

_@MichaReiser reviewed on 2025-08-05 10:51_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_close.rs`:62 on 2025-08-05 10:51_

We can add it to `SEssion::close_file`

---

_@dhruvmanila reviewed on 2025-08-05 10:55_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:57 on 2025-08-05 10:55_

I'm not sure if either of this is more beneficial mainly because we can only add the trace message once tracing is initialized and we require the initialization options (or the default ones) to initialize the tracing system. This is why I opted to return both the `Ok` and `Err` variant explicitly.

---

_@dhruvmanila reviewed on 2025-08-05 11:00_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:105 on 2025-08-05 11:00_

Do you mean the `CombineOptions` proc macro defined in `ruff_macros` ?

---

_@MichaReiser reviewed on 2025-08-05 11:06_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:105 on 2025-08-05 11:06_

No, it's `Combine. 

https://github.com/astral-sh/ruff/blob/fd335eb8b7065303193c5d3f04c76bca9da98b35/crates/ty_project/src/metadata/options.rs#L39

Thinking about this more. I think it may require you to move `Combine` into its own `ty_options` or `ty_combine` (or whatever) crate to make the macro work

---

_@dhruvmanila reviewed on 2025-08-05 11:19_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:229 on 2025-08-05 11:19_

I actually have a small doubt about this -- this is mainly to accommodate the fact that users _could_ set `diagnosticMode` to `workspace` in one workspace while `openFilesOnly` in other which isn't allowed but it's not something that we can enforce at the server level (we could probably check each workspace options before combining). So, the logic here states that `self` could either be `workspace` or `openFilesOnly` but if `other` is `workspace` then `self` should be `workspace`. This is to make sure that `workspace` always wins over `openFilesOnly`.

---

_@dhruvmanila reviewed on 2025-08-05 11:22_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:229 on 2025-08-05 11:22_

I've added a comment.

---

_@dhruvmanila reviewed on 2025-08-05 11:24_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:243 on 2025-08-05 11:24_

I think this path will only be taken when _both_ are `Some`, so I'm not sure what would "take the first value that is `Some`" look like. We could add a debug message and take the one from `self` but unless the user looks at the code (ty server or ty VS Code extension) they wouldn't know that something like this is possible.

---

_@dhruvmanila reviewed on 2025-08-05 11:26_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:332 on 2025-08-05 11:26_

Ah yes, we can use that, I did not see it was present in `ty_project`, I only saw the one in `ruff_server`.

---

_@dhruvmanila reviewed on 2025-08-05 11:29_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:70 on 2025-08-05 11:29_

Yeah, I've converted to `debug` for now but it might be useful to have the `display_settings` macro available by moving it out of `ruff_linter`.

---

_@dhruvmanila reviewed on 2025-08-05 11:31_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:77 on 2025-08-05 11:31_

That sounds reasonable. I've also moved the initialization options debug message after this so that it reads better and we can see whether the options were deserialized successfully or not before actually looking at the options:

Failed to deserialize ...
Initialization options: ...

---

_@dhruvmanila reviewed on 2025-08-05 11:32_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:462 on 2025-08-05 11:32_

Ah yes, I'm slowly getting used to the "combine" logic...

---

_@dhruvmanila reviewed on 2025-08-05 11:34_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:542 on 2025-08-05 11:34_

Yeah, I think that's better and now that I realize it it's more inline with some of the other values that are updated (e.g., on `File`).

---

_@dhruvmanila reviewed on 2025-08-05 11:37_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/snapshots/e2e__initialize__open_files_diagnostic_registration_via_initialization.snap`:1 on 2025-08-05 11:37_

Yeah, good idea, I'll update it

---

_@dhruvmanila reviewed on 2025-08-05 11:38_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:196 on 2025-08-05 11:38_

It's not a requirement for the storage part on the client side but it's a requirement when responding to the server which is handled by the `handle_workspace_configuration_request` method.

---

_@dhruvmanila reviewed on 2025-08-05 11:40_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:229 on 2025-08-05 11:40_

I'm using `.context`

---

_@dhruvmanila reviewed on 2025-08-05 11:47_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:105 on 2025-08-05 11:47_

Hmm, yeah. I'll move it to the `ty_combine` crate.

---

_Comment by @dhruvmanila on 2025-08-05 11:53_

(I've addressed most of the feedback, I need to address a couple more but I need to go out for sometime, will do it once I'm back.)

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-06 05:13_

---

_Review request for @carljm removed by @dhruvmanila on 2025-08-06 05:13_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-08-06 05:13_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-08-06 05:13_

---

_Comment by @github-actions[bot] on 2025-08-06 05:25_

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

_Merged by @dhruvmanila on 2025-08-06 13:07_

---

_Closed by @dhruvmanila on 2025-08-06 13:07_

---

_Branch deleted on 2025-08-06 13:07_

---
