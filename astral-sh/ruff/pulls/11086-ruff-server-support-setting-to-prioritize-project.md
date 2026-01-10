```yaml
number: 11086
title: "`ruff server`: Support setting to prioritize project configuration over editor configuration"
type: pull_request
state: merged
author: snowsignal
labels:
  - configuration
  - server
assignees: []
merged: true
base: main
head: jane/server/settings/prioritize-workspace-settings
created_at: 2024-04-22T01:38:43Z
updated_at: 2024-04-26T08:23:22Z
url: https://github.com/astral-sh/ruff/pull/11086
synced_at: 2026-01-10T22:37:01Z
```

# `ruff server`: Support setting to prioritize project configuration over editor configuration

---

_Pull request opened by @snowsignal on 2024-04-22 01:38_

## Summary

This is intended to address https://github.com/astral-sh/ruff-vscode/issues/425, and is a follow-up to https://github.com/astral-sh/ruff/pull/11062.

A new client setting is now supported by the server, `prioritizeFileConfiguration`. This is a boolean setting (default: `false`) that, if set to `true`, will instruct the configuration resolver to prioritize file configuration (aka discovered TOML files) over configuration passed in by the editor.

A corresponding extension PR has been opened, which makes this setting available for VS Code: https://github.com/astral-sh/ruff-vscode/pull/457.

## Test Plan

To test this with VS Code, you'll need to check out [the VS Code PR](https://github.com/astral-sh/ruff-vscode/pull/457) that adds this setting.

The test process is similar to https://github.com/astral-sh/ruff/pull/11062, but in scenarios where the editor configuration would take priority over file configuration, file configuration should take priority.


---

_Label `configuration` added by @snowsignal on 2024-04-22 01:38_

---

_Label `server` added by @snowsignal on 2024-04-22 01:38_

---

_Marked ready for review by @snowsignal on 2024-04-22 03:24_

---

_Comment by @github-actions[bot] on 2024-04-22 03:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/code_action.rs`:143 on 2024-04-24 02:33_

These getters are perhaps a nice example of something that could be put up as a trivial standalone PR that would be immediately approved and merged :)

---

_@charliermarsh reviewed on 2024-04-24 02:33_

---

_@charliermarsh reviewed on 2024-04-24 02:34_

I wonder if this should be an enum, rather than a bool? That way, we can associate documentation with each variant too, which seems like it would be helpful.

---

_Comment by @snowsignal on 2024-04-24 02:36_

> I wonder if this should be an enum, rather than a bool? That way, we can associate documentation with each variant too, which seems like it would be helpful.

@MichaReiser made a similar comment [here](https://github.com/astral-sh/ruff-vscode/pull/457#discussion_r1574236766). I think that's what I plan to go with :)

---

_Comment by @charliermarsh on 2024-04-24 02:36_

Oops, I swear I didn't see that!

---

_Comment by @snowsignal on 2024-04-24 02:43_

No worries 😄 

---

_@snowsignal reviewed on 2024-04-24 02:47_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action.rs`:143 on 2024-04-24 02:47_

Thanks, I'll keep that in mind for the future 👍 


---

_Review requested from @charliermarsh by @snowsignal on 2024-04-25 00:47_

---

_@charliermarsh reviewed on 2024-04-25 02:55_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-25 02:55_

I think we should have a name here that describes the strategy, rather than using `Default`.

---

_@charliermarsh reviewed on 2024-04-25 02:56_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:41 on 2024-04-25 02:56_

I'd like to find a way to make the name both shorter and more obvious to users. "Configuration preference"?

---

_@charliermarsh reviewed on 2024-04-25 02:57_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-25 02:57_

Could these be "Editor" and "Workspace" (or "Filesystem"), if the setting were called "configurationPreference"?

---

_@charliermarsh reviewed on 2024-04-25 02:57_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-25 02:57_

(I'd also suggest making it `#[default]`.)

---

_@charliermarsh reviewed on 2024-04-25 02:58_

The code looks good, I'm just not sold on the names...

---

_Comment by @charliermarsh on 2024-04-25 03:01_

I think @MichaReiser will have good advice on the naming and documentation.

---

_Comment by @MichaReiser on 2024-04-25 07:02_

> I think @MichaReiser will have good advice on the naming and documentation.

Lol no :laughing: I commented in the other PR that I don't have good naming suggestions haha

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:50 on 2024-04-25 07:04_

Nit: `Config` -> `Configuration` for consistency (you use configuration in most other places)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-25 07:05_

I like Charlie's suggestion. My first thought was `PrioritizeEditor`. I think I would not call it resolution, because I associate resolution with finding the configuration. Althought that's probably just me. 

An other possible name: `ConfigurationPrecedence`



---

_@MichaReiser approved on 2024-04-25 07:05_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-25 13:01_

Yeah I think I'd be fine approving either `ConfigurationPreference` or `ConfigurationPrecedence`, with options like:

- `Editor`
- `Filesystem`

Alternatively, if we _do_ want an option that uses the editor and _ignores_ workspace configuration (which is what I initially expected it to do), we could have:

- `EditorFirst` (equivalent to the `Default` here)
- `FilesystemFirst` (equivalent to `PrioritizeWorkspace` here)
- `EditorOnly` (use editor settings, ignore on-disk configuration)


---

_@charliermarsh reviewed on 2024-04-25 13:01_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:53 on 2024-04-26 07:58_

I like `ConfigurationPreference` and `EditorFirst`/`FilesystemFirst`/`EditorOnly` 👍 

---

_@snowsignal reviewed on 2024-04-26 07:58_

---

_Merged by @snowsignal on 2024-04-26 08:17_

---

_Closed by @snowsignal on 2024-04-26 08:17_

---

_Branch deleted on 2024-04-26 08:17_

---
