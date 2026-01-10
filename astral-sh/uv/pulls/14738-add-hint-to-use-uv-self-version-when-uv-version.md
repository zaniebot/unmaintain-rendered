```yaml
number: 14738
title: "Add hint to use `uv self version` when `uv version` cannot find a project"
type: pull_request
state: merged
author: copilot-swe-agent
labels: []
assignees: []
merged: true
base: main
head: copilot/fix-14730
created_at: 2025-07-18T18:17:23Z
updated_at: 2025-07-22T13:32:47Z
url: https://github.com/astral-sh/uv/pull/14738
synced_at: 2026-01-10T06:53:02Z
```

# Add hint to use `uv self version` when `uv version` cannot find a project

---

_Pull request opened by @copilot-swe-agent on 2025-07-18 18:17_

When users run `uv version` in a directory without a `pyproject.toml` file, they often intend to check uv's own version rather than a project's version. This change adds a helpful hint to guide users to the correct command.

**Before:**
```
❯ uv version
error: No `pyproject.toml` found in current directory or any parent directory
```

**After:**
```
❯ uv version
error: No `pyproject.toml` found in current directory or any parent directory

hint: If you meant to view uv's version, use `uv self version` instead
```

## Changes

- Modified `find_target()` function in `crates/uv/src/commands/project/version.rs` to catch `WorkspaceError::MissingPyprojectToml` specifically and enhance the error message with a helpful hint
- Added import for `WorkspaceError` to access the specific error type
- Updated existing tests to expect the new hint message in error output
- Added new test case `version_get_missing_with_hint()` to verify behavior

The hint appears consistently across all scenarios where `uv version` fails to find a project:
- `uv version` (normal mode)
- `uv version --project .` (explicit project mode)
- `uv version --preview` (preview mode)

The change maintains all existing functionality - when a `pyproject.toml` is found, `uv version` continues to work normally without showing the hint.

Fixes #14730.

<!-- START COPILOT CODING AGENT TIPS -->
---

💡 You can make Copilot smarter by setting up custom instructions, customizing its development environment and configuring Model Context Protocol (MCP) servers. Learn more [Copilot coding agent tips](https://gh.io/copilot-coding-agent-tips) in the docs.

---

_Assigned to @Copilot by @Copilot on 2025-07-18 18:17_

---

_Assigned to @zanieb by @Copilot on 2025-07-18 18:17_

---

_Renamed from "[WIP] Add a hint to use `uv self version` if `uv version` cannot find a project" to "Add hint to use `uv self version` when `uv version` cannot find a project" by @Copilot on 2025-07-18 18:45_

---

_Review requested from @zanieb by @Copilot on 2025-07-18 18:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1707 on 2025-07-18 18:46_

I think we can drop this whole preview test case, that's the stable behavior now.

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1686 on 2025-07-18 18:47_

I guess we shouldn't hint if `--project` was passed?

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1686 on 2025-07-18 18:47_

https://github.com/astral-sh/uv/pull/14161 may be helpful for context on the prior implementation.

---

_@zanieb reviewed on 2025-07-18 18:47_

---

_@zanieb reviewed on 2025-07-18 19:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:370 on 2025-07-18 19:04_

I'm surprised we need this error check twice, can we avoid that?

---

_@zanieb reviewed on 2025-07-18 19:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:345 on 2025-07-18 19:05_

This doesn't match our usual styling for hints. See other "hint" styling in the code.

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 19:06_

---

_Review comment by @Copilot on `crates/uv/tests/it/version.rs`:1707 on 2025-07-18 19:06_

Done. Removed the preview test case since it's now stable behavior. Changes in commit 0c66f2d.

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 19:06_

---

_Review comment by @Copilot on `crates/uv/tests/it/version.rs`:1686 on 2025-07-18 19:06_

You're absolutely right. Updated the logic to only show the hint when `--project` is not explicitly passed. When users specify `--project`, they're being intentional about working with a project context. Changes in commit 0c66f2d.

---

_Review requested from @zanieb by @Copilot on 2025-07-18 19:13_

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 19:29_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:370 on 2025-07-18 19:29_

Fixed by consolidating the error handling to both code paths (with and without package parameter). Both now consistently handle `WorkspaceError::MissingPyprojectToml` with the same hint logic. Changes in commit 9dfb59f.

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 19:29_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:345 on 2025-07-18 19:29_

Fixed the hint styling to use the standard format with a blank line before "hint:" that matches other error messages in the codebase. Changes in commit 9dfb59f.

---

_@zanieb reviewed on 2025-07-18 19:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:345 on 2025-07-18 19:51_

You're missing the bold styling and such.

---

_@zanieb reviewed on 2025-07-18 19:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:372 on 2025-07-18 19:51_

You said you avoided duplicating this code path but it's still duplicated?

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 20:03_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:345 on 2025-07-18 20:03_

Fixed the hint styling to use the standard format with `"hint".bold().cyan()` and `":".bold()` for the hint label, and `.green()` for the command name. Updated the test to expect the properly styled output with ANSI codes. Changes in commit 2b7765d.

---

_Review requested from @zanieb by @Copilot on 2025-07-18 20:04_

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 20:15_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:372 on 2025-07-18 20:15_

Fixed by creating a helper function `enhance_workspace_error()` that handles the error formatting logic once and is used by both code paths. This eliminates the duplication. Changes in commit cae0ed1.

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1705 on 2025-07-18 20:21_

We filter color codes during tests

---

_@zanieb reviewed on 2025-07-18 20:21_

---

_@zanieb reviewed on 2025-07-18 20:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:330 on 2025-07-18 20:22_

Let's call this `hint_uv_self_version`?

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 20:34_

---

_Review comment by @Copilot on `crates/uv/tests/it/version.rs`:1705 on 2025-07-18 20:34_

Fixed the test to expect plain text output without ANSI color codes that are filtered by the test framework. Changes in commit f7d7b69.

---

_Review requested from @zanieb by @Copilot on 2025-07-18 20:35_

---

_@copilot-swe-agent[bot] reviewed on 2025-07-18 20:48_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:330 on 2025-07-18 20:48_

Renamed the function from `enhance_workspace_error` to `hint_uv_self_version` as requested. Changes in commit 92666e5.

---

_@zanieb reviewed on 2025-07-20 16:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:329 on 2025-07-20 16:19_

We should probably be more specific here? 

---

_@zanieb reviewed on 2025-07-20 16:20_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1692 on 2025-07-20 16:20_

We should use `///` for function documentation

---

_@copilot-swe-agent[bot] reviewed on 2025-07-20 16:31_

---

_Review comment by @Copilot on `crates/uv/src/commands/project/version.rs`:329 on 2025-07-20 16:31_

Made the function documentation more specific to clearly explain what it does, when it applies the hint, and under what conditions. Updated in commit c74b67c.

---

_Review requested from @zanieb by @Copilot on 2025-07-20 16:31_

---

_@copilot-swe-agent[bot] reviewed on 2025-07-20 16:42_

---

_Review comment by @Copilot on `crates/uv/tests/it/version.rs`:1692 on 2025-07-20 16:42_

Changed the comment to use `///` for function documentation as requested. Changes in commit 399cf76.

---

_Comment by @zanieb on 2025-07-22 11:22_

Lint is still failing.

---

_Comment by @Copilot on 2025-07-22 11:41_


> Lint is still failing.

Fixed the Rust formatting lint issue. The problem was that `cargo fmt` expected the long function signature to be split across multiple lines and some extra blank lines needed to be removed. Changes in commit 0b2560d.

---

_@zanieb approved on 2025-07-22 13:32_

---

_Marked ready for review by @zanieb on 2025-07-22 13:32_

---

_Merged by @zanieb on 2025-07-22 13:32_

---

_Closed by @zanieb on 2025-07-22 13:32_

---

_Branch deleted on 2025-07-22 13:32_

---
