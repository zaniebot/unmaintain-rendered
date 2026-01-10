```yaml
number: 16361
title: Notify users for invalid client settings
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-warn-invalid-settings
created_at: 2025-02-25T07:59:42Z
updated_at: 2025-02-27T08:31:47Z
url: https://github.com/astral-sh/ruff/pull/16361
synced_at: 2026-01-10T19:49:01Z
```

# Notify users for invalid client settings

---

_Pull request opened by @dhruvmanila on 2025-02-25 07:59_

## Summary

As mentioned in https://github.com/astral-sh/ruff/pull/16296#discussion_r1967047387

This PR updates the client settings resolver to notify the user if there are any errors in the config using a very basic approach. In addition, each error related to specific settings are logged.

This isn't the best approach because it can log the same message multiple times when both workspace and global settings are provided and they both are the same. This is the case for a single workspace VS Code instance.

I do have some ideas on how to improve this and will explore them during my free time (low priority):
* Avoid resolving the global settings multiple times as they're static
* Include the source of the setting (workspace or global?)
* Maybe use a struct (`ResolvedClientSettings` + `Vec<ClientSettingsResolverError>`) instead to make unit testing easier

## Test Plan

Using:
```jsonc
{
  "ruff.logLevel": "debug",
	
  // Invalid settings
  "ruff.configuration": "$RANDOM",
  "ruff.lint.select": ["RUF000", "I001"],
  "ruff.lint.extendSelect": ["B001", "B002"],
  "ruff.lint.ignore": ["I999", "F401"]
}
```

The error logs:
```
2025-02-27 12:30:04.318736000 ERROR Failed to load settings from `configuration`: error looking key 'RANDOM' up: environment variable not found
2025-02-27 12:30:04.319196000 ERROR Failed to load settings from `configuration`: error looking key 'RANDOM' up: environment variable not found
2025-02-27 12:30:04.320549000 ERROR Unknown rule selectors found in `lint.select`: ["RUF000"]
2025-02-27 12:30:04.320669000 ERROR Unknown rule selectors found in `lint.extendSelect`: ["B001"]
2025-02-27 12:30:04.320764000 ERROR Unknown rule selectors found in `lint.ignore`: ["I999"]
```

Notification preview:

<img width="470" alt="Screenshot 2025-02-27 at 12 29 06 PM" src="https://github.com/user-attachments/assets/61f41d5c-2558-46b3-a1ed-82114fd8ec22" />



---

_Label `server` added by @dhruvmanila on 2025-02-25 07:59_

---

_Renamed from "Expand ruff.configuration to allow inline config" to "Notify users for invalid client settings" by @dhruvmanila on 2025-02-25 07:59_

---

_Comment by @github-actions[bot] on 2025-02-25 08:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2025-02-27 07:02_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-27 07:02_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:421 on 2025-02-27 07:52_

As a user (mainly VS Code), I'd find it difficult to understand what client settings are because I never configured them explicitly. Can we say invalid Ruff extension settings or invalid Ruff LSP settings?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 07:54_

Is this an error message that we could log from within Ruff itself or does it only apply to a subset of settings? 

---

_@MichaReiser approved on 2025-02-27 07:54_

---

_@dhruvmanila reviewed on 2025-02-27 08:08_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:08_

I don't think Ruff has a tracing system configured as it uses a basic [`log`](https://docs.rs/log/latest/log/) crate. So, those logs might not show up in the editor log channel

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:421 on 2025-02-27 08:10_

We can say "Ruff received invalid settings from the editor. ..." to make it not specific to VS Code and LSP (implementation detail).

---

_@dhruvmanila reviewed on 2025-02-27 08:10_

---

_@MichaReiser reviewed on 2025-02-27 08:14_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:14_

Tracing seems to work. At least, I can' see logs for the `format_path` spans

```
[2025-02-27][09:13:09][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/ruff/scripts/knot_benchmark/src/benchmark/__init__.py
[2025-02-27][09:13:09][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/ruff/scripts/knot_benchmark/src/benchmark/run.py
[2025-02-27][09:13:09][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/ruff/scripts/knot_benchmark/src/benchmark/projects.py
[2025-02-27][09:13:09][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/ruff/scripts/knot_benchmark/src/benchmark/cases.py
```

---

_@dhruvmanila reviewed on 2025-02-27 08:17_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:17_

This would also mean that a message can only contain a single rule, so there'll be `n` number of messages where `n` are the total number of unknown rule selectors.

---

_@dhruvmanila reviewed on 2025-02-27 08:18_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:18_

I'm also a bit unsure if we should include a log message inside a `FromStr` impl. We also won't know which key did the selector belonged to, was it `lint.select`, `lint.extendSelect`?

---

_@dhruvmanila reviewed on 2025-02-27 08:19_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:19_

Oh, wait, you meant to put this directly in the resolution of `lint.select` inside Ruff. Let me check

---

_@dhruvmanila reviewed on 2025-02-27 08:20_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:20_

Hmm, actually no, I don't think that's possible as we need to resolve it on the server side.

---

_@MichaReiser reviewed on 2025-02-27 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:442 on 2025-02-27 08:22_

Okay, thanks for checking.

I'm mainly asking because it would be nice to have less server specific logging. 

It might be worth exploring if we should change all ruff `debug`, and `trace` usages to use tracing instead of `log` so that they'd be visible in the server too. But that's something for another day.

---

_Merged by @dhruvmanila on 2025-02-27 08:28_

---

_Closed by @dhruvmanila on 2025-02-27 08:28_

---

_Branch deleted on 2025-02-27 08:28_

---
