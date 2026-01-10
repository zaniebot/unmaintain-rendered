```yaml
number: 16617
title: "Server: Remove log notification for `printDebugInformation` command"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: micha/ruff-0.10
head: dhruv/remove-debug-log
created_at: 2025-03-11T04:38:03Z
updated_at: 2025-03-11T08:23:08Z
url: https://github.com/astral-sh/ruff/pull/16617
synced_at: 2026-01-10T19:49:01Z
```

# Server: Remove log notification for `printDebugInformation` command

---

_Pull request opened by @dhruvmanila on 2025-03-11 04:38_

## Summary

For context, the initial implementation started out by sending a log
notification to the client to include this information in the client
channel. This is a bit ineffective because it doesn't allow the client
to display this information in a more obvious way. In addition to that,
it isn't obvious from a users perspective as to where the information is
being printed unless they actually open the output channel.

The change was to actually return this formatted string that contains
the information and let the client handle how it should display this
information. For example, in the Ruff VS Code extension we open a split
window and show this information which is similar to what rust-analyzer
does.

The notification request was kept as a precaution in case there are
users who are actually utilizing this way. If they exists, it should a
minority as it requires the user to actually dive into the code to
understand how to hook into this notification. With 0.10, we're removing
the old way as it only clobbers the output channel with a long message.

fixes: #16225

## Test Plan

Tested it out locally that the information is not being logged to the
output channel of VS Code.

---

_Label `server` added by @dhruvmanila on 2025-03-11 04:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-11 04:38_

---

_Comment by @github-actions[bot] on 2025-03-11 04:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-11 07:45_

Thank you

As a note for myself for the changelog.

This is specific to the debug command. Before, the command used to log the information. Now, the command opens a new window in VS code. Other LSP clients would require their own custom handling for this command (e.g. they could just dump the JSON output)

---

_Comment by @dhruvmanila on 2025-03-11 08:22_

> (e.g. they could just dump the JSON output)

It might be useful to use the term "string output" instead of JSON if this is a user facing information.

---

_Merged by @dhruvmanila on 2025-03-11 08:23_

---

_Closed by @dhruvmanila on 2025-03-11 08:23_

---

_Branch deleted on 2025-03-11 08:23_

---
