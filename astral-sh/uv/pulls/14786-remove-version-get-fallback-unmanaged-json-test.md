```yaml
number: 14786
title: "Remove `version_get_fallback_unmanaged_json` test"
type: pull_request
state: merged
author: copilot-swe-agent
labels:
  - internal
assignees: []
merged: true
base: main
head: copilot/fix-14785
created_at: 2025-07-21T12:27:38Z
updated_at: 2025-07-21T14:17:07Z
url: https://github.com/astral-sh/uv/pull/14786
synced_at: 2026-01-10T06:53:02Z
```

# Remove `version_get_fallback_unmanaged_json` test

---

_Pull request opened by @copilot-swe-agent on 2025-07-21 12:27_

The `version_get_fallback_unmanaged_json` test was failing when running tests outside of a git checkout (e.g., from a release tarball) due to inconsistent behavior based on git availability.

The test had conditional logic that expected different outcomes depending on whether `git_version_info_expected()` returned true or false:
- In git checkouts: Expected failure with "The project is marked as unmanaged" error
- Outside git checkouts: Expected success with fallback behavior showing version info

However, the fallback behavior was removed in version 0.8.0, making this test obsolete. All other similar tests (`version_get_fallback_unmanaged`, `version_get_fallback_unmanaged_short`, `version_get_fallback_unmanaged_strict`) consistently expect failure when a project is marked as unmanaged, regardless of git availability.

This change removes the problematic test entirely, as suggested by @zanieb. All remaining version tests (51 total) continue to pass.

Fixes #14785.

<!-- START COPILOT CODING AGENT TIPS -->
---

💡 You can make Copilot smarter by setting up custom instructions, customizing its development environment and configuring Model Context Protocol (MCP) servers. Learn more [Copilot coding agent tips](https://gh.io/copilot-coding-agent-tips) in the docs.

---

_Assigned to @Copilot by @Copilot on 2025-07-21 12:27_

---

_Assigned to @zanieb by @Copilot on 2025-07-21 12:27_

---

_Renamed from "[WIP] Since 0.8.0, version_get_fallback_unmanaged_json fails outside of a git checkout" to "Remove failing test: version_get_fallback_unmanaged_json" by @Copilot on 2025-07-21 12:45_

---

_Review requested from @zanieb by @Copilot on 2025-07-21 12:46_

---

_Label `internal` added by @zanieb on 2025-07-21 13:19_

---

_Marked ready for review by @zanieb on 2025-07-21 13:19_

---

_Renamed from "Remove failing test: version_get_fallback_unmanaged_json" to "Remove `version_get_fallback_unmanaged_json` test" by @zanieb on 2025-07-21 13:19_

---

_Comment by @zanieb on 2025-07-21 13:46_

Fix the lint, silly bot.

---

_Comment by @Copilot on 2025-07-21 13:58_


> Fix the lint, silly bot.

Fixed the linting issue by removing the extra blank line that was left behind after removing the test. Formatting now passes. (09150ce)

---

_Merged by @zanieb on 2025-07-21 14:17_

---

_Closed by @zanieb on 2025-07-21 14:17_

---

_Branch deleted on 2025-07-21 14:17_

---
