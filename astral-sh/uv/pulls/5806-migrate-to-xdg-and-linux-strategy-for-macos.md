```yaml
number: 5806
title: Migrate to XDG and Linux strategy for macOS directories
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: release/3
head: charlie/xdg
created_at: 2024-08-06T01:49:03Z
updated_at: 2024-08-19T19:33:23Z
url: https://github.com/astral-sh/uv/pull/5806
synced_at: 2026-01-10T13:09:50Z
```

# Migrate to XDG and Linux strategy for macOS directories

---

_Pull request opened by @charliermarsh on 2024-08-06 01:49_

## Summary

This PR moves us to the Linux strategy for our global directories on macOS. We both feel on the team _and_ have received feedback (in Issues and Polls) that the `Application Support` directories are more intended for GUIs, and CLI tools are correct to respect the XDG variables and use the same directory paths on Linux and macOS.

Namely, we now use:

- `/Users/crmarsh/.local/share/uv/tools` (for tools)
- `/Users/crmarsh/.local/share/uv/python` (for Pythons)
- `/Users/crmarsh/.cache/uv` (for the cache)

The strategy is such that if the `/Users/crmarsh/Library/Application Support/uv` already exists, we keep using it -- same goes for `/Users/crmarsh/Library/Caches/uv`, so **it's entirely backwards compatible**.

If you want to force a migration to the new schema, you can run:

- `uv cache clean`
- `uv tool uninstall --all`
- `uv python uninstall --all`

Which will clean up the macOS-specific directories, paving the way for the above paths. In other words, once you run those commands, subsequent `uv` operations will automatically use the `~/.cache` and `~/.local` variants.

Closes https://github.com/astral-sh/uv/issues/4411.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-06 01:49_

---

_Review requested from @konstin by @charliermarsh on 2024-08-06 01:49_

---

_Label `configuration` added by @charliermarsh on 2024-08-06 01:49_

---

_@charliermarsh reviewed on 2024-08-06 01:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/uninstall.rs`:47 on 2024-08-06 01:49_

This is a little bit more manual than I'd like.

---

_@charliermarsh reviewed on 2024-08-06 01:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/uninstall.rs`:41 on 2024-08-06 01:49_

We clean up the directories, if empty, such that if you `uninstall --all`, we remove `/Users/crmarsh/Library/Application Support/uv/tools`, and then `/Users/crmarsh/Library/Application Support/uv` if _that_ directory is empty.

---

_Review comment by @konstin on `crates/uv-cache/src/cli.rs`:46 on 2024-08-06 08:19_

This needs a comment about existing only for backwards compatibility

---

_@konstin approved on 2024-08-06 08:21_

---

_@charliermarsh reviewed on 2024-08-06 14:02_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:309 on 2024-08-06 14:02_

I need to check this on Windows.

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-08-06 17:12_

---

_Comment by @charliermarsh on 2024-08-06 17:50_

We're gonna bundle this with v0.3.0. Technically we could ship it now, but it could impact users that rely on the cache location "manually" on macOS. We say that's _never_ safe to do in the docs, but this change is breaking in spirit, so lets just make it part of the next minor.

---

_Merged by @zanieb on 2024-08-19 19:33_

---

_Closed by @zanieb on 2024-08-19 19:33_

---

_Branch deleted on 2024-08-19 19:33_

---
