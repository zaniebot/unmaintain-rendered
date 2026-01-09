---
number: 14443
title: "Allow setting frozen=true or locked=true for `uv run` in pyproject.toml"
type: issue
state: open
author: benjsec
labels:
  - enhancement
assignees: []
created_at: 2025-07-03T16:29:49Z
updated_at: 2025-10-08T10:36:24Z
url: https://github.com/astral-sh/uv/issues/14443
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow setting frozen=true or locked=true for `uv run` in pyproject.toml

---

_Issue opened by @benjsec on 2025-07-03 16:29_

### Summary

After switching to UV we were a little surprised with the default behaviour of uv updating the lockfile implicitly when running any command. It goes against some of our workflows, where we aim to keep dependency updates (including transient dependencies) separate from code changes where possible.
We've been able to use the environment variable UV_FROZEN to disable this behaviour in CI, but we can't find a good way of enforcing it on developers machines.

Could you add a way to set uv_frozen=true in the pyproject.toml so we can avoid inadvertently upgrading dependencies, please?

### Example

_No response_

---

_Label `enhancement` added by @benjsec on 2025-07-03 16:29_

---

_Comment by @zanieb on 2025-07-03 16:51_

> default behaviour of uv updating the lockfile implicitly when running any command

Can you share some more details about when dependencies are changing? The lockfile should not change unless someone has changed the dependencies, asked for an explicit upgrade, or otherwise invalidated the lockfile.

---

_Comment by @benjsec on 2025-07-08 08:50_

The documentation at https://docs.astral.sh/uv/concepts/projects/sync/#automatic-lock-and-sync says
> Locking and syncing are automatic in uv. For example, when uv run is used, the project is locked and synced before invoking the requested command.

I have confirmed this with
```
➜  uv lock --check
Resolved 157 packages in 2.36s
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
➜  uv run ruff
      Built api @ file:///Users/ben/Documents/Projects/api
Uninstalled 29 packages in 298ms
Installed 28 packages in 51ms
➜  uv lock --check
Resolved 157 packages in 9ms
```

Running `uv run ruff` has updated the lockfile automatically

---

_Renamed from "Allow setting frozen=true or locked=true in pyproject.toml" to "Allow setting frozen=true or locked=true for `uv run` in pyproject.toml" by @konstin on 2025-08-01 08:56_

---

_Comment by @rbebb on 2025-10-06 18:54_

It would be great to have this functionality.

When working on projects with a team, we want to ensure consistency across each dev's environment. Having the lock file update every time someone runs `uv run` can lead to confusion if an issue arises since it's not immediately clear if a dependency update caused it or a code change caused it.

---

_Comment by @helpmefindaname on 2025-10-08 10:36_

Same here,
I personally disagree with the default values, so being able to change them for `--frozen` and similar (like `--no-sync`, `--locked`) in the pyproject.toml or uv.toml on a project level would be very helpful 

---
