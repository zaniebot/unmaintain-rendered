```yaml
number: 82
title: LSP client settings
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:27:01Z
updated_at: 2025-08-07T14:03:23Z
url: https://github.com/astral-sh/ty/issues/82
synced_at: 2026-01-12T15:54:22Z
```

# LSP client settings

---

_@MichaReiser_

Similar to Ruff's LSP. Support setting some (or all) settings from within the LSP client. Allow setting LSP specific settings.

---

_Label `server` added by @MichaReiser on 2025-03-14 10:27_

---

_Renamed from "[red-knot] LSP settings" to "[red-knot] LSP client settings" by @MichaReiser on 2025-03-14 10:27_

---

_Renamed from "[red-knot] LSP client settings" to "LSP client settings" by @MichaReiser on 2025-05-07 15:16_

---

_Comment by @dhruvmanila on 2025-05-30 08:56_

There's some additional context at https://github.com/astral-sh/ty/issues/282 around using pull model for configuration instead of the current way which relies on initialization options.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-06-04 06:19_

---

_Comment by @dhruvmanila on 2025-07-02 06:24_

Looking through https://github.com/astral-sh/ruff/pull/18984, I went on a bit of a rabbit hole:

After reading a couple of issues / PRs mentioned in https://github.com/microsoft/language-server-protocol/issues/567#issuecomment-2085131917, it's a bit ambiguous on what the server needs to do exactly but here's what I've gathered so far:
- The `InitializationOptions` should only contain the options that are static while the server is active e.g., any command-line options because any change to these settings require a complete restart of the server
  - For us these would be the `logLevel` and `logFile` options for now because we cannot update the global tracing subscriber ([ref](https://docs.rs/tracing/0.1.41/tracing/subscriber/fn.set_global_default.html))
- Initialize each workspace with the default options during `initialize`
  - Should we read the option values under the `ty`, `python` and any other namespaces from `InitializationOptions` ? I don't think we should as that would be fetched via the `workspace/configuration` request
  - This would mean that the user doesn't need to provide the server settings via the `initialization_options` (name might differ from editor to editor) and can directly provide via the generic settings mechanism for that editor
  - But, reading through https://rust-analyzer.github.io/book/configuration.html, it seems that some editor might not support `workspace/configuration` so the users are forced to specify them via `InitializationOptions`. Looking at the editors page, it seems to be [Emacs via eglot](https://rust-analyzer.github.io/book/other_editors.html#eglot), [Vim via vim-lsp](https://rust-analyzer.github.io/book/other_editors.html#vim-lsp), [Kate texte editor](https://rust-analyzer.github.io/book/other_editors.html#kate-text-editor) although I haven't confirmed whether it's still the same as of today (2025-07-02). So, we _might_ still want to read the initialization options.
- The server should fetch the settings for a specific namespace (e.g., `ty`, `python`) for all the workspaces after `initialized` via the `workspace/configuration` request
  - The server can still process any requests / notifications while the server is waiting for the configuration response from the clients but we will opt into locking the server to defer processing these notifications or requests. This is what this PR is also doing.
- When the server receives the `workspace/didChangeConfiguration` notification, the server should refresh the settings from all the managed workspaces, ignoring any values in the notification payload.
- The server should update the internal cache of workspaces when it receives the `workspace/didChangeWorkspaceFolders` as:
    - For all the added workspace folders, update the internal state to include the new folder and fetch the settings for this folder
    - For all the removed workspace folders, remove any internal state related to this workspace


---

_Comment by @dhruvmanila on 2025-07-07 14:10_

As per [this conversation](https://github.com/astral-sh/ty-vscode/pull/65#discussion_r2182409136), we also need to explicitly register for the `didChangeConfiguration` notification. According to the spec:

> This pull model replaces the old push model were the client signaled configuration change via an event. If the server still needs to react to configuration changes (since the server caches the result of workspace/configuration requests) the server should register for an empty configuration change using the following registration pattern:
>
> https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration

---

_Comment by @MichaReiser on 2025-07-13 14:16_

For scoping this feature: We should focus on the essential functionality. We can implement better configuration support in the future (e.g. it would be fine if the server restarts on every configuration change). I also suggest that we start with a narrow set of settings that we extend over time.

---

_Comment by @MichaReiser on 2025-08-07 10:39_

@dhruvmanila should we close this one and track the remaining work in the sub tasks?

---

_Comment by @dhruvmanila on 2025-08-07 11:09_

> should we close this one and track the remaining work in the sub tasks?

Yes, I'll create separate issues for the last two bullet points from above and then close this one. Thanks for the reminder.

---

_Comment by @dhruvmanila on 2025-08-07 14:03_

Going to mark this as completed, I've opened follow-up issues.

---

_Closed by @dhruvmanila on 2025-08-07 14:03_

---
