```yaml
number: 13854
title: "\"uv python pin --global\" Should Automatically Use Latest or Current Project Version"
type: issue
state: open
author: zyads
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-06-05T05:18:09Z
updated_at: 2025-06-05T16:04:57Z
url: https://github.com/astral-sh/uv/issues/13854
synced_at: 2026-01-10T03:41:47Z
```

# "uv python pin --global" Should Automatically Use Latest or Current Project Version

---

_Issue opened by @zyads on 2025-06-05 05:18_

### Summary

When using the uv python pin --global command, it currently requires a specific Python version to be provided. As opposed to showing an error: "No pinned Python version found," it would be more user-friendly if the command could automatically determine and use the latest available Python version or the version currently selected in the current project (if a .python-version file exists).

Steps to Reproduce
1. Run uv python pin --global without specifying a version.
2. Observe the error message: "No pinned Python version found."

Expected Behavior
- If no version is specified, uv python pin --global should:
  - Check for the latest available Python version and use that.
  - Alternatively, if a .python-version file exists in the current project, use the version specified in that file.

Actual Behavior
- The command fails with the error message "No pinned Python version found" and requires a specific version to be provided.

System Information
UV Version: 0.7.11
Python Version: 3.13
Operating System: Linux

Related Documentation
[UV Python Version Management](https://docs.astral.sh/uv/concepts/python-versions/)
[UV Install Python Guide](https://docs.astral.sh/uv/guides/install-python/)

Thank you for considering this enhancement!

### Example

- Use Case: Users often want to set a global Python version that matches their current project or the latest available version without having to manually specify it each time.
- Proposed Solution:
  - Add a flag to automatically determine the latest version or use the current project's version.
  - Example: uv python pin --global --latest to use the latest available version.
  - Example: uv python pin --global --set-current-to-global to use the version specified in the current project's .python-version file.

---

_Label `enhancement` added by @zyads on 2025-06-05 05:18_

---

_Comment by @zanieb on 2025-06-05 14:28_

I'm not sure I agree we should set a global pin based on local context without an explicit value

---

_Label `needs-decision` added by @zanieb on 2025-06-05 14:28_

---

_Comment by @zanieb on 2025-06-05 16:04_

I think we should just add a call to action: https://github.com/astral-sh/uv/pull/13862

---
