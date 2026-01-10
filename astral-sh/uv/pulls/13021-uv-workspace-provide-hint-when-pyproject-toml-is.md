```yaml
number: 13021
title: "uv-workspace: provide hint when pyproject.toml is missing"
type: pull_request
state: open
author: eduardorittner
labels: []
assignees: []
base: main
head: missing-pyproject-err
created_at: 2025-04-21T19:35:35Z
updated_at: 2025-04-21T21:58:28Z
url: https://github.com/astral-sh/uv/pull/13021
synced_at: 2026-01-10T11:10:40Z
```

# uv-workspace: provide hint when pyproject.toml is missing

---

_Pull request opened by @eduardorittner on 2025-04-21 19:35_

## Summary

When `pyproject.toml` is missing, add a hint in the error message that tells the user to run `uv init`. We don't link to the documentation since it's not versioned and thus not guaranteed to be stable.

## Test Plan

Since the change is pretty simple, all I did was update snapshot tests that had the old error message with `cargo insta review`.

## Related

Closes https://github.com/astral-sh/uv/issues/12988


---

_Comment by @eduardorittner on 2025-04-21 21:32_

I'm not sure if the `hint` is done correctly, should I change `WorkspaceError` to contain a hint on the `MissingPyprojectToml` case? And then implement `Display` for it, formatting the hint correctly with cyan.

---

_Marked ready for review by @eduardorittner on 2025-04-21 21:39_

---

_Assigned to @zanieb by @zanieb on 2025-04-21 21:58_

---
