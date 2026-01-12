```yaml
number: 8724
title: "Add `uv sync --all` and `uv run --all`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-10-31T13:44:23Z
updated_at: 2024-11-02T01:55:10Z
url: https://github.com/astral-sh/uv/issues/8724
synced_at: 2026-01-12T15:59:33Z
```

# Add `uv sync --all` and `uv run --all`

---

_@charliermarsh_

To sync the entire workspace.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-31 13:44_

---

_Label `enhancement` added by @charliermarsh on 2024-10-31 13:44_

---

_Comment by @zanieb on 2024-10-31 14:15_

Should we do like `--all-members` or `--all-packages`? Or is that just verbose for no reason? Is there something else someone that's not working in a workspace context would think `--all` means?

---

_Comment by @jackvreeken on 2024-10-31 14:27_

Probably related, but it would then also be nice to have `uv export --all` . I think that `uv build` already has an `--all` option to "build all packages in the workspace", as far as consistency of naming is concerned. Might clash a little with the `--all-*` (extras, platforms, versions) that some other commands like `run` have.

---

_Comment by @samypr100 on 2024-10-31 23:17_

How about an explicit `--workspace` to imply all?

---

_Comment by @charliermarsh on 2024-11-01 00:28_

I don't mind a `--workspace` alias, I think I advocated for that with `uv build` though and was overruled :)

---

_Closed by @charliermarsh on 2024-11-02 01:55_

---

_Closed by @charliermarsh on 2024-11-02 01:55_

---
