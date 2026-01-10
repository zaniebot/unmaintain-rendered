```yaml
number: 8733
title: Improve interactions with existing Python executables during install
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/python-exec
created_at: 2024-10-31T18:27:59Z
updated_at: 2024-11-04T20:22:45Z
url: https://github.com/astral-sh/uv/pull/8733
synced_at: 2026-01-10T11:59:59Z
```

# Improve interactions with existing Python executables during install

---

_Pull request opened by @zanieb on 2024-10-31 18:27_

Previously, we'd use the `--reinstall` flag to determine if we should replace existing Python executables in the bin directory during an install. There are a few problems with this:

- We replace executables we don't manage
- We can replace executables from other uv Python installations during reinstall (surprising)
- We don't do the "right" thing when installing patch versions e.g. installing `3.12.4` then `3.12.6` would fail without the reinstall flag

In `uv tool`, we have separate `--force` and `--reinstall` concepts. Here we separate the flags (`--force` was previously just a `--reinstall` alias) and add inspection of the existing executables to inform a decision on replacement.

In brief, we will:

- Replace any executables with `--force`
- Replace executables for the same installation with `--reinstall`
- Replace executables for an older patch version by default


---

_Label `preview` added by @zanieb on 2024-10-31 18:28_

---

_Marked ready for review by @zanieb on 2024-10-31 19:20_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-31 20:26_

---

_@charliermarsh reviewed on 2024-11-02 17:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:313 on 2024-11-02 17:18_

I think we'd typically not include a period here, since it's a single sentence

---

_@charliermarsh reviewed on 2024-11-02 17:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:321 on 2024-11-02 17:18_

We may want to use `simplified_display` for these which is always absolute, but don't feel strongly.

---

_@charliermarsh reviewed on 2024-11-02 17:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:324 on 2024-11-02 17:18_

Does this resolve symlinks? (Does it need to? Should we use same-file?)

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:373 on 2024-11-02 17:19_

Can we not do an atomic replace here?

---

_@charliermarsh reviewed on 2024-11-02 17:19_

---

_@charliermarsh reviewed on 2024-11-02 17:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:364 on 2024-11-02 17:20_

Should any of this be user-facing? I'm unsure... At least this last one seems like it should be, though? Otherwise we risk silent no-ops here.

---

_@zanieb reviewed on 2024-11-02 17:33_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:373 on 2024-11-02 17:33_

Hm yeah we can probably do an atomic replace as in `uv-fs::replace_symlink`. That doesn't change here, it's just refactored, so I'd prefer to tackle it separately.

---

_@charliermarsh approved on 2024-11-03 02:51_

(I trust you to resolve feedback.)

---

_@zanieb reviewed on 2024-11-04 19:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:324 on 2024-11-04 19:27_

No, but I can't think of a reasonable case where it needs to. I don't think we can use `same-file` because it's a directory.

---

_@zanieb reviewed on 2024-11-04 19:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:364 on 2024-11-04 19:28_

I'm not sure either. I opted for this to be silent because if I ran `uv python install 3.12.6` then `uv python install 3.12.4` I would not expect the `python3.12` executable to change.

---

_Comment by @zanieb on 2024-11-04 20:18_

I'll follow up on that atomic replacement and we can iterate on the level of the logs based on feedback from preview users.

---

_Merged by @zanieb on 2024-11-04 20:22_

---

_Closed by @zanieb on 2024-11-04 20:22_

---

_Branch deleted on 2024-11-04 20:22_

---
