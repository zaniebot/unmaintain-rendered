```yaml
number: 11062
title: "`ruff server`: Ruff configuration from client settings overrides project configuration"
type: pull_request
state: merged
author: snowsignal
labels:
  - configuration
  - server
assignees: []
merged: true
base: main
head: jane/server/settings/resolution
created_at: 2024-04-21T01:24:39Z
updated_at: 2024-04-23T18:19:18Z
url: https://github.com/astral-sh/ruff/pull/11062
synced_at: 2026-01-12T15:55:34Z
```

# `ruff server`: Ruff configuration from client settings overrides project configuration

---

_@snowsignal_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/10984 that implements configuration resolution for editor configuration. By 'editor configuration', I'm referring to the client settings that correspond to Ruff configuration/options, like `preview`, `select`, and so on. These will be combined with 'project configuration' (configuration taken from project files such as `pyproject.toml`) to generate the final linter and formatter settings used by `RuffSettings`. Editor configuration takes priority over project configuration.

In a follow-up pull request, I'll implement a new client setting that allows project configuration to override editor configuration, as per [this issue](https://github.com/astral-sh/ruff-vscode/issues/425).

## Review guide

The first commit, e38966d8843becc7234fa7d46009c16af4ba41e9, is just doing re-arrangement so that we can pass the right things to `RuffSettings::resolve`. The actual resolution logic is in the second commit, 0eec9ee75c10e5ec423bd9f5ce1764f4d7a5ad86. It might help to look at these comments individually since the diff is rather messy.

## Test Plan

For the settings to show up in VS Code, you'll need to checkout this branch: https://github.com/astral-sh/ruff-vscode/pull/456.

To test that the resolution for a specific setting works as expected, run through the following scenarios, setting it in project and editor configuration as needed:

| Set in project configuration? | Set in editor configuration?                     | Expected Outcome                                                                         |
|-------------------------------|--------------------------------------------------|------------------------------------------------------------------------------------------|
| No                            | No                                               | The editor should behave as if the setting was set to its default value.                  |
| Yes                           | No                                               | The editor should behave as if the setting was set to the value in project configuration. |
| No                            | Yes                                              | The editor should behave as if the setting was set to the value in editor configuration.  |
| Yes                           | Yes (but distinctive from project configuration) | The editor should behave as if the setting was set to the value in editor configuration.  |

An exception to this is `extendSelect`, which does not have an analog in TOML configuration. Instead, you should verify that `extendSelect` amends the `select` setting. If `select` is set in both editor and project configuration, `extendSelect` will only append to the `select` value in editor configuration, so make sure to un-set it there if you're testing `extendSelect` with `select` in project configuration.

---

_Label `configuration` added by @snowsignal on 2024-04-21 01:24_

---

_Label `server` added by @snowsignal on 2024-04-21 01:24_

---

_Comment by @github-actions[bot] on 2024-04-21 01:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @snowsignal on 2024-04-21 23:24_

---

_Comment by @snowsignal on 2024-04-22 01:32_

I'm working on adding some snapshot tests, but the PR should be ready for review regardless.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:74 on 2024-04-22 07:59_

Have you considered implementing `ConfigurationTransformer` for `ResolvedEditorSettings`. It's our current mechanism for transforming configurations and is used by the CLI to merge the CLI arguments with the project configuration. See `ConfigArguments`. You could then continue using `resolve_root_settings`. 

It would also remove the need for the empty `LSPConfigTransformer` (or implement the logic on `LSPConfigTransformer`)

---

_@MichaReiser reviewed on 2024-04-22 08:01_

---

_@snowsignal reviewed on 2024-04-22 15:15_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:74 on 2024-04-22 15:15_

Oh, good catch!

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-22 20:50_

---

_@MichaReiser approved on 2024-04-23 07:11_

Thank you

---

_@dhruvmanila approved on 2024-04-23 08:28_

---

_Comment by @snowsignal on 2024-04-23 18:19_

I'll implement snapshot tests [as a follow-up](https://github.com/astral-sh/ruff/issues/11112)

---

_Merged by @snowsignal on 2024-04-23 18:19_

---

_Closed by @snowsignal on 2024-04-23 18:19_

---

_Branch deleted on 2024-04-23 18:19_

---
