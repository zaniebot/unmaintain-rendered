```yaml
number: 13657
title: Add version bump github action
type: issue
state: open
author: zkurtz
labels:
  - enhancement
assignees: []
created_at: 2025-05-26T12:21:41Z
updated_at: 2025-07-16T04:22:33Z
url: https://github.com/astral-sh/uv/issues/13657
synced_at: 2026-01-10T03:32:45Z
```

# Add version bump github action

---

_Issue opened by @zkurtz on 2025-05-26 12:21_

uv recently added the `uv version --bump` command. I'm proposing to wrap this in a convenient GitHub Action, similar to how [phips28/gh-action-bump-version](https://github.com/phips28/gh-action-bump-version) works for npm projects.

Scope: Version management operations like:
- Check if version needs bumping
- Bump version (default to patch; minor+ would require harder logic)
- Commit and push changes
- Optional git tagging

Would you encourage the community creating this, or prefer it stays as manual uv version commands in workflows?

I see some related work/discussion around this, but can't tell how much these complement vs eliminate the need for a github action:
- https://github.com/alltuner/uv-version-bumper
- https://github.com/astral-sh/uv/issues/5903 - `uv run` as a task runner


---

_Label `enhancement` added by @zkurtz on 2025-05-26 12:21_

---

_Comment by @cthoyt on 2025-07-16 00:49_

@zkurtz I made a similar issue that's just about the commit/pushing changes and optional git tagging in #14644 

---

_Comment by @zanieb on 2025-07-16 04:22_

I don't think we'll be able to guide something like this officially in the short-term.

---
