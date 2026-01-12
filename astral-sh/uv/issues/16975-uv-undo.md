```yaml
number: 16975
title: Uv undo
type: issue
state: open
author: SimonVervisch
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-12-03T20:15:29Z
updated_at: 2026-01-12T11:34:16Z
url: https://github.com/astral-sh/uv/issues/16975
synced_at: 2026-01-12T16:02:41Z
```

# Uv undo

---

_@SimonVervisch_

### Summary

possible to return to the last 5 environments for example? It would be nice to have a snapshot.

uv undo

> do you really want to change back

> yes
> changes back to the old version

### Example

The issue I had, is that one, is that a new pip install changed versions of other packages. 

I preferred the old version, but I think I cannot look back at that one. 

Thank you

---

_Label `enhancement` added by @SimonVervisch on 2025-12-03 20:15_

---

_Comment by @zanieb on 2025-12-03 20:48_

In general, I'd recommend using version control and a lockfile for this purpose. But it's a fun idea!

---

_Label `wish` added by @zanieb on 2025-12-03 20:48_

---

_Comment by @notatallshaw on 2025-12-04 16:56_

As prior art conda keeps a full revision history and allows you to restore to any previous revision: https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/manage-environments.html#restoring-an-environment

---

_Comment by @CarliJoy on 2026-01-12 11:34_

This is similar to #13217, which proposes tracking lockfile changes in the VCS.

I wrote a small wrapper, https://pypi.org/project/uvrev/, that already implements this approach.

However, I do not think we should implement a `uv undo` command. The required semantics would be too complex.

Instead, as outlined in #13217, uv should provide an option that automatically commits `pyproject.toml`, `uv.lock`, and `uv.toml`, and tags the commit with a revision number whenever `uv.lock` changes. Restoring and listing historical `uv.lock` states would then be handled using standard Git tooling (`git log`, `git checkout`, followed by `uv sync`).

This approach also increases flexibility in how the feature can be used.

If we were to add an explicit undo mechanism, we would either need to maintain a separate Git history for `uv.lock` changes or make assumptions about the structure of a project’s Git history (for example, a project might update `uv.lock` during a release or security check without invoking `uv` directly).

For these reasons, I suggest closing this issue in favor of #13217.


---
