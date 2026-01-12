```yaml
number: 14790
title: Copy entry points and Jupyter data directories into ephemeral environments
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/eph-fix
created_at: 2025-07-21T14:40:18Z
updated_at: 2025-07-22T12:11:06Z
url: https://github.com/astral-sh/uv/pull/14790
synced_at: 2026-01-12T16:11:25Z
```

# Copy entry points and Jupyter data directories into ephemeral environments

---

_@zanieb_

This is an alternative to https://github.com/astral-sh/uv/pull/14788 which has the benefit that it addresses https://github.com/astral-sh/uv/issues/13327 which would be an issue even if we reverted #14447.

There are two changes here

1. We copy entry points into the ephemeral environment, and rewrite their shebangs (or trampoline target) to ensure the ephemeral environment is not bypassed.
2. We link `etc/jupyter` and `share/jupyter` data directories into the ephemeral environment, this is in order to ensure the above doesn't break Jupyter which unfortunately cannot find the `share` directory otherwise. I'd love not to do this, as it seems brittle and we don't have a motivating use-case beyond Jupyter. I've opened https://github.com/jupyterlab/jupyterlab/issues/17716 upstream for discussion, as there is a viable patch that could be made upstream to resolve the problem. I've limited the fix to Jupyter directories so we can remove it without breakage.

Closes https://github.com/astral-sh/uv/issues/14729
Closes https://github.com/astral-sh/uv/issues/13327
Closes https://github.com/astral-sh/uv/issues/14749

---

_Marked ready for review by @zanieb on 2025-07-21 19:03_

---

_@zanieb reviewed on 2025-07-21 19:04_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:44 on 2025-07-21 19:04_

In order to allow rewriting the launcher, we need to stash the payload. I think this is generally pretty small. If we are worried about the overhead, we could store the path to the original launcher and load it again when needed.

---

_@zanieb reviewed on 2025-07-21 19:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1100 on 2025-07-21 19:44_

Since this is just for Jupyter right now, we could narrow this to just apply to the `share/jupyter` directory if we wanted 🤔 

---

_@charliermarsh reviewed on 2025-07-21 22:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1755 on 2025-07-21 22:03_

I think we also need to check for absolute shebangs, because we use these in the `--with` environment, but we use standard absolute shebangs in the base environment.

---

_@zanieb reviewed on 2025-07-21 22:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1755 on 2025-07-21 22:42_

We do, that's what https://github.com/astral-sh/uv/blob/d477e368c9c6c7d0c59c1362c11be14846ce7ff1/crates/uv/src/commands/project/run.rs#L1760 does (and there's test coverage for both)

---

_Renamed from "Copy entry points and common data directories into ephemeral environments" to "Copy entry points and Jupyter data directories into ephemeral environments" by @zanieb on 2025-07-21 22:53_

---

_@zanieb reviewed on 2025-07-21 22:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1100 on 2025-07-21 22:55_

I narrowed this

---

_@zanieb reviewed on 2025-07-21 22:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1755 on 2025-07-21 22:57_

I updated it to be clearer

---

_@charliermarsh reviewed on 2025-07-21 23:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1781 on 2025-07-21 23:14_

Is this included by mistake? There's another call just below.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1785 on 2025-07-21 23:15_

I think this overwrites if the file already exists?

---

_@charliermarsh reviewed on 2025-07-21 23:15_

---

_@charliermarsh reviewed on 2025-07-21 23:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1168 on 2025-07-21 23:15_

Is this intended to be merged? It seems like it's showing the path to the scripts, not the env.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1103 on 2025-07-21 23:16_

Why a shallow merge, rather than linking the entire `etc/jupyter` or `share/jupyter` dir?

---

_@charliermarsh reviewed on 2025-07-21 23:16_

---

_@zanieb reviewed on 2025-07-21 23:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1781 on 2025-07-21 23:23_

Uh yeah sorry totally missed that.

---

_@zanieb reviewed on 2025-07-21 23:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1785 on 2025-07-21 23:23_

True. I guess we can't do something atomic here? We need to just check existence?

---

_@zanieb reviewed on 2025-07-21 23:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1168 on 2025-07-21 23:24_

Sorry this was from your commit and I didn't read it closely when changing it to a debug log, I guess I'll update it to print the root path?

---

_@zanieb reviewed on 2025-07-21 23:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1103 on 2025-07-21 23:25_

That's a good question. I think we can remove the shallow merge now that I'm not doing `etc` and `share` — previously the shallow merge seemed pretty important.

---

_@charliermarsh reviewed on 2025-07-21 23:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1168 on 2025-07-21 23:30_

Oh haha. I'd just remove it, I assume this is logged elsewhere.

---

_@charliermarsh reviewed on 2025-07-21 23:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1785 on 2025-07-21 23:30_

I think you can do it with `fs_err::OpenOptions::new()`. There's an option to error if it already exists when you open the file.

---

_Comment by @zanieb on 2025-07-21 23:34_

Thanks for the review, addressed in https://github.com/astral-sh/uv/pull/14790/commits/7883b0bb2ecf14c9efa5c8c725a7da62ce73796b

---

_@zanieb reviewed on 2025-07-21 23:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1785 on 2025-07-21 23:40_

Ah that's nice, thanks!

---

_Label `bug` added by @zanieb on 2025-07-22 00:07_

---

_@charliermarsh reviewed on 2025-07-22 01:47_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:167 on 2025-07-22 01:47_

I know this used to exist in some form, can you just double-check the Git history to make sure this matches anything we learned from our prior implementation?

---

_@charliermarsh reviewed on 2025-07-22 01:48_

---

_Review comment by @charliermarsh on `crates/uv-trampoline-builder/src/lib.rs`:44 on 2025-07-22 01:48_

If you want to get fancy you could probably store `Box<[u8]>` here. Seems totally not necessary though.

---

_@zanieb reviewed on 2025-07-22 01:49_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:167 on 2025-07-22 01:49_

Yeah sorry this is a verbatim copy of `replace_symlink` without deletion of a junction. I'll add a comment saying so.

---

_@zanieb reviewed on 2025-07-22 01:51_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:167 on 2025-07-22 01:51_

Or are you asking something else?

---

_@charliermarsh reviewed on 2025-07-22 01:52_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1771 on 2025-07-22 01:52_

This is really minor, but I think the relative shebang includes a trailing newline whereas this one does not. That might just mean that we're adding an extra newline in the absolute case?

---

_@charliermarsh reviewed on 2025-07-22 01:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 01:56_

I can't remember why we have this in the wheel installer, I need to look:
```rust
let permissions = fs::metadata(&script_absolute)?.permissions();
if permissions.mode() & 0o111 != 0o111 {
    fs::set_permissions(
        script_absolute,
        Permissions::from_mode(permissions.mode() | 0o111),
    )?;
}
```

---

_@zanieb reviewed on 2025-07-22 01:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 01:58_

Oo spicy. That's interesting.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1771 on 2025-07-22 01:58_

I can just snapshot them and make sure they're consistent.

---

_@zanieb reviewed on 2025-07-22 01:58_

---

_@charliermarsh reviewed on 2025-07-22 01:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 01:59_

Oh, nevermind, I guess it's just because we might be able to skip setting permissions. Why `perms.set_mode(0o755);` though?

---

_@charliermarsh reviewed on 2025-07-22 02:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 02:00_

I think you maybe want to drop this. Doesn't this override the permissions entirely?

---

_@charliermarsh reviewed on 2025-07-22 02:15_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:167 on 2025-07-22 02:15_

Na that's fine, thanks.

---

_@zanieb reviewed on 2025-07-22 02:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 02:16_

I copied this from your commit. It makes sense though, we're creating a new file, don't we need to make it executable?

---

_@charliermarsh reviewed on 2025-07-22 02:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 02:24_

I think you want to copy the permissions from the existing file though, like:
```rust
let mode = fs_err::metadata(source)?.permissions().mode();
let mut file = fs_err::OpenOptions::new()
    .create_new(true)
    .write(true)
    .mode(mode)
    .open(target)?;
```

(That code from my branch was just a hack.)

---

_@zanieb reviewed on 2025-07-22 03:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 03:09_

That seems quite reasonable.

---

_@zanieb reviewed on 2025-07-22 03:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 03:10_

74ad1b5d9

---

_@charliermarsh approved on 2025-07-22 11:58_

---

_Merged by @zanieb on 2025-07-22 12:11_

---

_Closed by @zanieb on 2025-07-22 12:11_

---

_Branch deleted on 2025-07-22 12:11_

---
