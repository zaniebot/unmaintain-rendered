```yaml
number: 10764
title: "Implement client setting initialization and resolution for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/settings/init-options
created_at: 2024-04-03T20:53:02Z
updated_at: 2024-04-05T23:26:38Z
url: https://github.com/astral-sh/ruff/pull/10764
synced_at: 2026-01-12T15:55:33Z
```

# Implement client setting initialization and resolution for `ruff server`

---

_@snowsignal_

## Summary

When a language server initializes, it is passed a serialized JSON object, which is known as its "initialization options". Until now, `ruff server` has ignored those initialization options, meaning that user-provided settings haven't worked. This PR is the first step for supporting settings from the LSP client. It implements procedures to deserialize initialization options into a settings object, and then resolve those settings objects into concrete settings for each workspace.

One of the goals for user settings implementation in `ruff server` is backwards compatibility with `ruff-lsp`'s settings. We won't support all settings that `ruff-lsp` had, but the ones that we do support should work the same and use the same schema as `ruff-lsp`.

These are the existing settings from `ruff-lsp` that we will continue to support, and which are part of the settings schema in this PR:

| Setting                                | Default Value | Description                                                                            |
|----------------------------------------|---------------|----------------------------------------------------------------------------------------|
| `codeAction.disableRuleComment.enable` | `true`        | Whether to display Quick Fix actions to disable rules via `noqa` suppression comments. |
| `codeAction.fixViolation.enable`       | `true`        | Whether to display Quick Fix actions to autofix violations.                            |
| `fixAll`                               | `true`        | Whether to register Ruff as capable of handling `source.fixAll` actions.               |
| `lint.enable`                          | `true`        | Whether to enable linting. Set to `false` to use Ruff exclusively as a formatter.      |
| `organizeImports`                      | `true`        | Whether to register Ruff as capable of handling `source.organizeImports` actions.      |

To be clear: this PR does not implement 'support' for these settings, individually. Rather, it constructs a framework for these settings to be used by the server in the future.

Notably, we are choosing *not* to support `lint.args` and `format.args` as settings for `ruff server`. This is because we're now interfacing with Ruff at a lower level than its CLI, and converting CLI arguments back into configuration is too involved.

We will have support for linter and formatter specific settings in follow-up PRs. We will also 'hook up' user settings to work with the server in follow up PRs.

## Test Plan

### Snapshot Tests

Tests have been created in `crates/ruff_server/src/session/settings/tests.rs` to ensure that deserialization and settings resolution works as expected.

### Manual Testing

Since we aren't using the resolved settings anywhere yet, we'll have to add a few printing statements.

We want to capture what the resolved settings look like when sent as part of a snapshot, so modify `Session::take_snapshot` to be the following:

```rust
    pub(crate) fn take_snapshot(&self, url: &Url) -> Option<DocumentSnapshot> {
        let resolved_settings = self.workspaces.client_settings(url, &self.global_settings);
        tracing::info!("Resolved settings for document {url}: {resolved_settings:?}");
        Some(DocumentSnapshot {
            configuration: self.workspaces.configuration(url)?.clone(),
            resolved_client_capabilities: self.resolved_client_capabilities.clone(),
            client_settings: resolved_settings,
            document_ref: self.workspaces.snapshot(url)?,
            position_encoding: self.position_encoding,
            url: url.clone(),
        })
    }
```

Once you've done that, build the server and start up your extension testing environment.

1. Set up a workspace in VS Code with two workspace folders, each one having some variant of Ruff file-based configuration (`pyproject.toml`, `ruff.toml`, etc.). We'll call these folders `folder_a` and `folder_b`.
2. In each folder, open up `.vscode/settings.json`.
3. In folder A, use these settings:
```json
{
    "ruff.codeAction.disableRuleComment": {
        "enable": true
    }
}
```
4. In folder B, use these settings:
```json
{
    
    "ruff.codeAction.disableRuleComment": {
        "enable": false
    }
}
```
5. Finally, open up your VS Code User Settings and un-check the `Ruff > Code Action: Disable Rule Comment` setting.
6. When opening files in `folder_a`, you should see logs that look like this:
```
Resolved settings for document <file>: ResolvedClientSettings { fix_all: true, organize_imports: true, lint_enable: true, disable_rule_comment_enable: true, fix_violation_enable: true }
```
7. When opening files in `folder_b`, you should see logs that look like this:
```
Resolved settings for document <file>: ResolvedClientSettings { fix_all: true, organize_imports: true, lint_enable: true, disable_rule_comment_enable: false, fix_violation_enable: true }
```
8. To test invalid configuration, change `.vscode/settings.json` in either folder to be this:
```json
{
    "ruff.codeAction.disableRuleComment": {
        "enable": "invalid"
    },
}
```
10. You should now see these error logs:
```
<time> [info]    <duration> ERROR ruff_server::session::settings Failed to deserialize initialization options: data did not match any variant of untagged enum InitializationOptions. Falling back to default client settings...

<time> [info]    <duration> WARN ruff_server::server No workspace settings found for file:///Users/jane/testbed/pandas
   <duration> WARN ruff_server::server No workspace settings found for file:///Users/jane/foss/scipy
```
11. Opening files in either folder should now print the following configuration:
```
Resolved settings for document <file>: ResolvedClientSettings { fix_all: true, organize_imports: true, lint_enable: true, disable_rule_comment_enable: true, fix_violation_enable: true }
```

---

_Label `server` added by @snowsignal on 2024-04-03 20:53_

---

_Comment by @github-actions[bot] on 2024-04-03 21:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-04-04 08:07_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:50 on 2024-04-04 08:18_

Nit: This is unrelated to your PR but I was confused why `Session` accepts `server_capabilities`. It seems the only reason is to extract the `position_encoding`. I wonder if we should instead:

* Have a dedicated function to compute the `position_encoding`: `let position_encoding = choose_position_encoding(&client_capabilities)`
* Pass the position encoding to `server_capabilities`: `Self::server_capabilities(&client_capabilities, position_encoding)`;
* Pass the position encoding to `Session`


---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:40 on 2024-04-04 08:19_

Does Rust consider it dead code because there's no read outside of tests?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings/tests.rs`:1 on 2024-04-04 08:21_

Nit: I would be okay having this inline of `settings.rs`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:16 on 2024-04-04 08:24_

I would find it helpful to document what the key of `workspace_settings` is. Is it keyed by the workspace root URL? If so, how do we ensure that workspace removal removes the settings from the `workspace_settings` map (how do we avoid stale entries)?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:69 on 2024-04-04 08:25_

I'm generally a fan of newtype wrappers but I think I prefer a simple bool here. There's just too much boilerplate involved and there are so many boolean fields and the benefits of the newtype wrappers here isn't clear to me (I like `RunWhen`)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:199 on 2024-04-04 08:28_

Would it make sense to use an enum for `self` that encodes that fact as well that the settings are either: Global only, or Global with workspace settings. I think it would make the distinction more clear (I was very confused why we only log the message when the workspace isn't empty)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:242 on 2024-04-04 08:29_

NIt: I would rename this to `resolve_impl`, `resolve_value` or even make it a free standing function. The `_` prefix convention is used to mark allowed unused methods, which isn't what we have here.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:214 on 2024-04-04 08:32_

What I understand is that we can have at most two settings: 
* the global settings
* an optional workspace settings

I think I would encode this explicitly in the API (you can still use a slice internally if it simplifies the implementation)


```suggestion
    fn resolve(global_settings: UserSettings, workspace_settings: Option<&UserSettings>) -> Self {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:39 on 2024-04-04 08:33_

I find the name `UserSettings` somewhat confusing because VS Code has the concept of user settings. User settings in VS code are the settings that apply to all workspaces (they are probably stored in your home directory?). 

Should we name this ClientSettings OR extension settings? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:244 on 2024-04-04 08:34_

I know it requires a bit more code to deref the value but passing `&&T` feels strange in a function argument

```suggestion
        get: impl Fn(&UserSettings) -> Option<T>,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:54 on 2024-04-04 08:36_

Would it be possible to recover from this error or show users a helpful error message? Can you demonstrate in your test plan on how this looks?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings/tests.rs`:29 on 2024-04-04 08:55_

To me, the tests in here have a too high complexity. I don't feel like I can jump into fixing an issue because:

* The macro requires me to learn a DSL just to understand the tests. This adds cognitive overhead
* The test, the input and expected output are spread across multiple files.

I think we should try simplifying the logic so that tests become easy to read and maintain, especially because there's not that much repetition. 

That's why I think I would use

* Helper functions for deserializing (even if it requires repeating one line)
* Helper functions that define recurring test input (can use `include_str` if the input is large or define the input inline)
* Using insta snapshots to assert the deserialization output (inline or out of file depending on the snapshot size)


```rust
 use insta::assert_debug_snapshot;
use super::*;

mod fixtures;

fn deserialize_initialization_options(json: &str) -> InitializationOptions {
    serde_json::from_str(json).expect("test fixture JSON should deserialize")
}

fn vs_code_initialization_options() -> InitializationOptions {
    deserialize_initialization_options(include_str!("./tests/fixtures/vs_code_initialization_options.json"))
}

fn global_only_options() -> InitializationOptions {
    deserialize_initialization_options(r#"{
        "settings": {
            "codeAction": {
                "disableRuleComment": {
                    "enable": false
                }
            },
            "lint": {
                "run": "onSave"
            },
            "fixAll": false,
            "logLevel": "warn"
        }
    }"#)
}

#[test]
fn test_vs_code_init_options_deserialize() {
    let options = vs_code_initialization_options();

    assert_debug_snapshot!(options);
}

#[test]
fn test_vs_code_workspace_settings_resolve() {
    let options = vs_code_initialization_options();
    let settings_map = SettingsController::from_init_options(options);
    assert_eq!(
        settings_map.settings_for_workspace(Path::new("/Users/test/projects/pandas")),
        ResolvedUserSettings {
            fix_all: FixAll(true),
            organize_imports: OrganizeImports(true),
            lint_enable: LintEnable(true),
            run: RunWhen::OnType,
            disable_rule_comment_enable: CodeActionEnable(false),
            fix_violation_enable: CodeActionEnable(false),
            log_level: LogLevel::Error,
        }
    );
    assert_eq!(
        settings_map.settings_for_workspace(Path::new("/Users/test/projects/scipy")),
        ResolvedUserSettings {
            fix_all: FixAll(true),
            organize_imports: OrganizeImports(true),
            lint_enable: LintEnable(true),
            run: RunWhen::OnType,
            disable_rule_comment_enable: CodeActionEnable(true),
            fix_violation_enable: CodeActionEnable(false),
            log_level: LogLevel::Error,
        }
    );
}

#[test]
fn test_vs_code_unknown_workspace_resolves_to_fallback() {
    let options = vs_code_initialization_options();

    let settings_map = SettingsController::from_init_options(options);
    assert_eq!(
        settings_map.settings_for_workspace(Path::new("/Users/test/invalid/workspace")),
        ResolvedUserSettings {
            fix_all: FixAll(false),
            organize_imports: OrganizeImports(true),
            lint_enable: LintEnable(true),
            run: RunWhen::OnType,
            disable_rule_comment_enable: CodeActionEnable(false),
            fix_violation_enable: CodeActionEnable(false),
            log_level: LogLevel::Error,
        }
    );
}

#[test]
fn test_global_only_init_options_deserialize() {
    let options = global_only_options();

    assert_debug_snapshot!(options, @r###"
    GlobalOnly {
        settings: Some(
            UserSettings {
                fix_all: Some(
                    FixAll(
                        false,
                    ),
                ),
                organize_imports: None,
                lint: Some(
                    Lint {
                        enable: None,
                        run: Some(
                            OnSave,
                        ),
                    },
                ),
                code_action: Some(
                    CodeAction {
                        disable_rule_comment: Some(
                            CodeActionSettings {
                                enable: Some(
                                    CodeActionEnable(
                                        false,
                                    ),
                                ),
                            },
                        ),
                        fix_violation: None,
                    },
                ),
                log_level: Some(
                    Warn,
                ),
            },
        ),
    }
    "###);
}

#[test]
fn test_global_only_resolves_correctly() {
    let options = global_only_options();

    let settings_map = SettingsController::from_init_options(options);
    assert_eq!(
        settings_map.settings_for_workspace(Path::new("/Users/test/some/workspace")),
        ResolvedUserSettings {
            fix_all: FixAll(false),
            organize_imports: OrganizeImports(true),
            lint_enable: LintEnable(true),
            run: RunWhen::OnSave,
            disable_rule_comment_enable: CodeActionEnable(false),
            fix_violation_enable: CodeActionEnable(true),
            log_level: LogLevel::Warn,
        }
    );
}

#[test]
fn test_empty_init_options_deserialize() {
    let empty = deserialize_initialization_options("{}");

    assert_debug_snapshot!(empty, @r###"
    GlobalOnly {
        settings: None,
    }
    "###);
}
```

---

_@MichaReiser requested changes on 2024-04-04 09:04_

I would prefer if we exclude `lint.run` from this PR. What I understood from @charliermarsh's comment in your settings proposal document is that the setting was added to address a performance issue with Ruff's LSP. I would like first to verify if this is still an issue with the new LSP before adding support for the setting. 

`logLevel`: I don't know if the LSP must know about this setting because it is only used today to set up tracing. We should remove it EXCEPT if it is necessary to correctly deserialize the settings when the field is set in the VS Code configuration.

Should the settings struct support all fields of today's LSP or what happens if VS code sends the settings of the LSP today? Will it just fail to start? Can we design this more gracefully (can be something for a follow up PR). 

I like that you didn't implement the logic to respect the settings in this PR. However, I would like a test plan showing that initialization is working. The unit tests show that the deserialization works but it doesn't demonstrate that the settings are correctly loaded during initialization. One thing we could do is to trace the settings (resolved settings?) during startup with a debug log level, similar to what the LSP does today. This should be easy to implement but would demonstrate the functionality.

I think the current implementation doesn't correctly recycle workspace configurations when workspaces are added and removed. It might be worth exploring a data structure design where this happens automatically (e.g., by storing the configuration on the workspace). 

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:88 on 2024-04-04 09:15_

Maybe `RunTrigger`?

We could also move the `Default` implementation here from down below.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:150 on 2024-04-04 09:20_

This is unrelated to the PR but I want to understand how are these log messages being sent to the client. Currently, we use `tracing` which I think just dumps the messsages to stderr which is what the client is picking up?

It's not required to be done in this PR but it would be useful to move to providing useful messages to the user using either of the following:
1. [Show message notification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_showMessage)
2. [Show message request](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_showMessageRequest)
3. [Log message](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_logMessage)

For `ruff-lsp`, we use a combination of (2) and (3) depending on user config and the log level.

---

_@dhruvmanila reviewed on 2024-04-04 09:25_

---

_Comment by @snowsignal on 2024-04-04 22:25_

@MichaReiser:

> I would prefer if we exclude lint.run from this PR. What I understood from @charliermarsh's comment in your settings proposal document is that the setting was added to address a performance issue with Ruff's LSP. I would like first to verify if this is still an issue with the new LSP before adding support for the setting.

Your reasoning makes sense, I'll leave this off for now.

> `logLevel`: I don't know if the LSP must know about this setting because it is only used today to set up tracing. We should remove it EXCEPT if it is necessary to correctly deserialize the settings when the field is set in the VS Code configuration.

I'll take this one out for now, since we do need some better planning around logging/tracing for the server. Also, I don't think the VS Code extension even supports this setting at the moment.

> Should the settings struct support all fields of today's LSP or what happens if VS code sends the settings of the LSP today? Will it just fail to start? Can we design this more gracefully (can be something for a follow up PR).

We've talked about this elsewhere but by default, `serde_json` allows deserialization from a struct with additional fields. You can see this in our unit tests for VS Code configuration, which passes in extraneous fields that we don't need and aren't using.

> I think the current implementation doesn't correctly recycle workspace configurations when workspaces are added and removed. It might be worth exploring a data structure design where this happens automatically (e.g., by storing the configuration on the workspace).

I agree that stale configuration from removed workspaces is an issue and I'll work to address that in this PR, though adding new workspace settings when a workspace is added is going to require extension-side work, so that's a future problem to solve. The good news is that this issue will be transient, and can be fixed by a server reload if anyone runs into it.

---

_@snowsignal reviewed on 2024-04-04 22:28_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:150 on 2024-04-04 22:28_

Right now, we using the `tracing` library for logging, which writes out to `stderr`. This does show up in the `Output` window in the editor, though it's always tagged as `[info]` and it would be nice to have log messages tagged properly. Switching over to use `window/logMessage` has actually been a side project that I've made some progress on this week. I hope to open some PRs in the near future.

---

_@snowsignal reviewed on 2024-04-04 22:29_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:88 on 2024-04-04 22:29_

We might actually remove this setting for the time being (see [Micha's comment](https://github.com/astral-sh/ruff/pull/10764#pullrequestreview-1979073417))

---

_@snowsignal reviewed on 2024-04-04 22:30_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:40 on 2024-04-04 22:30_

Right. This will be used in follow-up PRs.

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-05 07:00_

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-05 07:00_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:450 on 2024-04-05 07:30_

Is it intentional that we don't assert to what the empty options deserialize to? Like we could add a `assert_eq(deserialized, InitialzationOptions::default())` to at least compare to something.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:214 on 2024-04-05 07:37_

Nit: the fact that the macro concatenates the path breaks some IDE functionality. For example, JetBrain IDEs allow me to click on the argument passed to `include_str!` and it opens the file for me. However, that only works when the path is static. That's why I still think not using a macro here is preferred:

```rust
fn deserialize_fixture<T>(content: &str) -> T {
    serde_json::from_str(content).expect("test fixture JSON should deserialize")
}
```

and inside tests you can either write

```rust
let options: InitializationOptions =
            deserialize_fixture(include_str!("../../resources/test/fixtures/settings/vs_code_initialization_options.json"));

// or 
let options: InitializationOptions =
            deserialize_fixture(include_str!("{}"));
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:16 on 2024-04-05 07:39_

Nit: Can we add a TODO to remove the `dead_code` warning

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:151 on 2024-04-05 07:44_

Nit: It's possible to avoid some of the `and_then` by using the try operator (I'm okay with `and_then` if that's what you prefer)
```suggestion
                |settings| settings.lint.as_ref()?.enable,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:162 on 2024-04-05 07:44_


```suggestion
                    settings
                        .code_action
                        .as_ref()?
                        .disable_rule_comment
                        .as_ref()?
                        .enable
                },
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:173 on 2024-04-05 07:45_


```suggestion
                |settings| settings.code_action.as_ref()?.fix_violation?.enable,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:242 on 2024-04-05 07:51_

Nit: can we create an issue to track this that outlines the problem (and possible options)?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:293 on 2024-04-05 07:52_

Nit: Maybe `client_settings` to more clearly distinguish it from Ruff settings.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:94 on 2024-04-05 07:55_

Nit: I don't know if this is unintentional or your persoanl preference. I prefer to have the impl block follow the `struct` definition because it allows me to read through the code from top to bottom without having to jump between struct definition and implementations and I think that's what we generally do in the Ruff code base

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:8 on 2024-04-05 07:55_

Nit: Can we document what the `key` in the map is?

---

_@MichaReiser approved on 2024-04-05 07:58_

Thanks, this looks good. 

Could you please update your test plan and show that the settings are correctly passed from VS code? 

> I like that you didn't implement the logic to respect the settings in this PR. However, I would like a test plan showing that initialization is working. The unit tests show that the deserialization works but it doesn't demonstrate that the settings are correctly loaded during initialization. One thing we could do is to trace the settings (resolved settings?) during startup with a debug log level, similar to what the LSP does today. This should be easy to implement but would demonstrate the functionality.

Edit: I just saw that there's now a manual test plan. Consider this as resolved.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:137 on 2024-04-05 08:26_

nit: I leave this up to you but I find the `_only` prefix redundant, I'd probably remove it

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:190 on 2024-04-05 08:31_

Can we add documentation for `resolve_or`? What I understand is that it applies the `get` function for all `ClientSettings` in the _order_ provided, falling back to `default`. This means that the order of `all_settings` is important here which we can mention in the documentation.

Separately, does this function need to be part of the `impl` block or can it be a standalone helper function scoped to this module? I'm fine with either way.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:214 on 2024-04-05 08:37_

> For example, JetBrain IDEs allow me to click on the argument passed to `include_str!` and it opens the file for me. However, that only works when the path is static.

Neovim also has a similar functionality where you can press a key (`gf`) and it will take you to that file.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:65 on 2024-04-05 08:45_

I don't think this should be a warning, it should be at info or debug. Neovim doesn't have the concept of workspaces builtin so it will always default to providing it as global settings.

---

_@dhruvmanila approved on 2024-04-05 08:46_

Thank you! 

I tested it on Neovim, provided some logs for the same which prints the resolved settings as mentioned in the PR test plan:

```
[ERROR][2024-04-05 14:11:24] .../vim/lsp/rpc.lua:789	"rpc"	"/Users/dhruv/work/astral/ruff-test/target/release/ruff"	"stderr"	"┘\n   0.008890s INFO ruff_server::session Resolved settings for document file:///Users/dhruv/playground/ruff/src/lsp.py: ResolvedClientSettings { fix_all: false, organize_imports: false, lint_enable: false, disable_rule_comment_enable: false, fix_violation_enable: false }\n"
[ERROR][2024-04-05 14:13:15] .../vim/lsp/rpc.lua:789	"rpc"	"/Users/dhruv/work/astral/ruff-test/target/release/ruff"	"stderr"	"   0.045634s INFO ruff_server::session Resolved settings for document file:///Users/dhruv/playground/ruff/src/lsp.py: ResolvedClientSettings { fix_all: false, organize_imports: false, lint_enable: false, disable_rule_comment_enable: true, fix_violation_enable: false }\n"
```

---

_@snowsignal reviewed on 2024-04-05 22:04_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:450 on 2024-04-05 22:04_

No, that was an oversight. Let's add that.

---

_@snowsignal reviewed on 2024-04-05 22:09_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:151 on 2024-04-05 22:09_

I agree that the try operator looks cleaner here.

---

_@snowsignal reviewed on 2024-04-05 22:15_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:94 on 2024-04-05 22:15_

I usually prefer to group struct definitions at the top and then put all the `impl`s below - I think it makes the struct definitions themselves easier to read, and I think it makes sense for the heavily-nested structs here. What I can do, though, is move `AllSettings` down to be closer to its associated `impl` block.

---

_@snowsignal reviewed on 2024-04-05 22:18_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:190 on 2024-04-05 22:18_

The function doesn't have to be part of the `impl` block, but I'd prefer to keep it that way.

---

_Merged by @snowsignal on 2024-04-05 22:41_

---

_Closed by @snowsignal on 2024-04-05 22:41_

---

_Branch deleted on 2024-04-05 22:41_

---

_@snowsignal reviewed on 2024-04-05 23:26_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:242 on 2024-04-05 23:26_

Issue created here: https://github.com/astral-sh/ruff/issues/10797

---
