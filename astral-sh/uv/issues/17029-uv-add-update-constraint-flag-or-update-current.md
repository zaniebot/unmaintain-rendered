```yaml
number: 17029
title: uv add --update-constraint flag or update current upgrade flag
type: issue
state: closed
author: ThomasLamalle
labels:
  - enhancement
assignees: []
created_at: 2025-12-08T10:52:51Z
updated_at: 2025-12-13T03:04:42Z
url: https://github.com/astral-sh/uv/issues/17029
synced_at: 2026-01-12T16:02:42Z
```

# uv add --update-constraint flag or update current upgrade flag

---

_@ThomasLamalle_

### Summary


Summary:
Add a flag to uv add that updates the version constraint in pyproject.toml when upgrading a package to the latest available version.

Current Behavior:
When using uv add --upgrade package, uv resolves and installs the latest compatible version and updates uv.lock, but does not update the version constraint in pyproject.toml. The declared constraint remains unchanged (e.g., package >= 0.5.3 even though 0.6.3 is now installed).

This creates a mismatch between:

The declared constraint in pyproject.toml  (what developers see)
The actual version in uv.lock (what's being used)
Desired Behavior:
Option 1: Add a new flag like uv add --upgrade --update-constraint package that would:

Resolve to the latest version available
Update uv.lock with the new version
Update pyproject.toml to reflect the new constraint (e.g., >= 0.6.3)

Option 2: Update the existing --upgrade flag behavior to also modify pyproject.toml by default:

When uv add --upgrade package is used, automatically update the version constraint in pyproject.toml to match the resolved version, not just uv.lock
This would make the behavior more intuitive and consistent with the user's intent to upgrade

Option 3: Add a new command like uv update package that does this by default, as a more explicit way to update both files.

Use Case:
When explicitly upgrading a dependency to its latest version, developers want pyproject.toml to reflect this intent, ensuring the constraint stays current and prevents accidental downgrades in future installations.

Current Workaround:
Manual editing of pyproject.toml or using uv add 'package>=0.6.3' (requires knowing the exact version).

### Example

_No response_

---

_Label `enhancement` added by @ThomasLamalle on 2025-12-08 10:52_

---

_Comment by @charliermarsh on 2025-12-13 03:04_

Ah yeah, unfortunately `uv add` doesn't upgrade existing bounds right now. I think it'd be best to track https://github.com/astral-sh/uv/issues/6794 -- we want to add a dedicated uv upgrade command or flow, it's on the near-term roadmap.

---

_Closed by @charliermarsh on 2025-12-13 03:04_

---

_Comment by @charliermarsh on 2025-12-13 03:04_

(Agree we need this, just merging with https://github.com/astral-sh/uv/issues/6794.)

---
