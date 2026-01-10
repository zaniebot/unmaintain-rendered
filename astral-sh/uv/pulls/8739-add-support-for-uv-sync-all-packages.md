```yaml
number: 8739
title: "Add support for `uv sync --all-packages`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2024-11-01T00:19:35Z
updated_at: 2024-11-03T16:03:01Z
url: https://github.com/astral-sh/uv/pull/8739
synced_at: 2026-01-10T11:59:59Z
```

# Add support for `uv sync --all-packages`

---

_Pull request opened by @charliermarsh on 2024-11-01 00:19_

## Summary

This PR enables `uv sync --all-packages` to sync all packages in a workspace. It removes a common use-case for the legacy non-`[project]` packages that we're trying to move away from.

Closes https://github.com/astral-sh/uv/issues/8724.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-01 00:19_

---

_Review requested from @konstin by @charliermarsh on 2024-11-01 00:19_

---

_Label `enhancement` added by @charliermarsh on 2024-11-01 00:19_

---

_Comment by @charliermarsh on 2024-11-01 13:55_

I would vote to add a `--workspace` alias to these or even use `--workspace` as the default name.

---

_Renamed from "Add support for `uv sync --all`" to "Add support for `uv sync --all-packages`" by @charliermarsh on 2024-11-02 01:45_

---

_Merged by @charliermarsh on 2024-11-02 01:55_

---

_Closed by @charliermarsh on 2024-11-02 01:55_

---

_Branch deleted on 2024-11-02 01:55_

---

_Comment by @konstin on 2024-11-03 15:32_

fwiw, cargo renamed `--all` to `--workspace` (https://github.com/rust-lang/cargo/issues/6977):

> I have noticed that people occasionally get confused with the --all flag, usually confusing it with the --all-targets flag. I'd like to propose renaming it to --workspace which should better convey what the flag is for.

`--all-packages` should disambiguate this for us.

---

_Comment by @bluss on 2024-11-03 16:03_

If I'd be the devil's advocate / annoying guy, 'package' is *a bit* overloaded and people will find a way to be confused.

Just for an outside perspective read `uv help sync` and categorize the two different senses of 'package' used.

Existing descriptions are relatively consistent and that's good. Maybe I would say that `uv help sync` uses both 'package in the workspace' and 'workspace members' terminology, in different places, and it's not clear if these are the same.

---
