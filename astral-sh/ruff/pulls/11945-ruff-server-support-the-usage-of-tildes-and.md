```yaml
number: 11945
title: "`ruff server`: Support the usage of tildes and environment variables in `logFile`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/config/logfile-shellexpand
created_at: 2024-06-20T02:53:07Z
updated_at: 2024-06-20T19:01:48Z
url: https://github.com/astral-sh/ruff/pull/11945
synced_at: 2026-01-10T21:56:00Z
```

# `ruff server`: Support the usage of tildes and environment variables in `logFile`

---

_Pull request opened by @snowsignal on 2024-06-20 02:53_

## Summary

Fixes #11911.

`shellexpand` is now used on `logFile` to expand the file path, allowing the usage of `~` and environment variables.

## Test Plan

1. Set `logFile` in either Neovim or Helix to a file path that needs expansion, like `~/.config/helix/ruff_logs.txt`.
2. Ensure that `RUFF_TRACE` is set to `messages` or `verbose`
3. Open a Python file in Neovim/Helix
4. Confirm that a file at the path specified was created, with the expected logs.


---

_Label `server` added by @snowsignal on 2024-06-20 02:53_

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-20 02:53_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-06-20 02:53_

---

_Renamed from "`ruff server`: The `logFile` setting now supports expanded paths" to "`ruff server`: Support the usage of tildes and environment variables in `logFile`" by @snowsignal on 2024-06-20 02:53_

---

_Comment by @github-actions[bot] on 2024-06-20 03:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:88 on 2024-06-20 04:04_

nit: logs for Neovim are usually stored under `~/.local/state/nvim`, you can find out the path by running `:echo stdpath('log')` in Neovim :)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:65 on 2024-06-20 04:08_

If not already done, let's document the fact that the path in `logFile` is expanded if it contained `~` (tilde) or any environment variables.

---

_@dhruvmanila approved on 2024-06-20 04:11_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:86 on 2024-06-20 06:38_

I think this is worth mentioning in the user facing documentation

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:73 on 2024-06-20 06:39_

Unrelated to this change: I think it would be good to write to stderr if opening the logfile fails instead of failing silently

---

_@MichaReiser approved on 2024-06-20 06:39_

---

_@dhruvmanila reviewed on 2024-06-20 07:43_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:65 on 2024-06-20 07:43_

Sorry for not being clear. I meant to document it in user-facing docs.

---

_Merged by @snowsignal on 2024-06-20 18:51_

---

_Closed by @snowsignal on 2024-06-20 18:51_

---

_Branch deleted on 2024-06-20 18:51_

---
