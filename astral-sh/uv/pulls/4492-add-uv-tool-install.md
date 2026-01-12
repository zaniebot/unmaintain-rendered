```yaml
number: 4492
title: "Add `uv tool install`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-install
created_at: 2024-06-24T21:09:01Z
updated_at: 2024-06-26T15:25:55Z
url: https://github.com/astral-sh/uv/pull/4492
synced_at: 2026-01-12T16:06:15Z
```

# Add `uv tool install`

---

_@zanieb_

This is the minimal "working" implementation. In summary, we:

- Resolve the requested requirements
- Create an environment at `$UV_STATE_DIR/tools/$name`
- Inspect the `dist-info` for the main requirement to determine its entry points scripts
- Link the entry points from a user-executable directory (`$XDG_BIN_HOME`) to the environment bin
- Create an entry at `$UV_STATE_DIR/tools/tools.toml` tracking the user's request

The idea with `tools.toml` is that it allows us to perform upgrades and syncs, retaining the original user request (similar to declarations in a `pyproject.toml`). I imagine using a similar schema in the `pyproject.toml` in the future if/when we add project-levle tools. I'm also considering exposing `tools.toml` in the standard uv configuration directory instead of the state directory, but it seems nice to tuck it away for now while we iterate on it. Installing a tool won't perform a sync of other tool environments, we'll probably have an explicit `uv tool sync` command for that? 

I've split out todos into follow-up pull requests:

- #4509 (failing on Windows)
- #4501 
- #4504 

Closes #4485 

---

_Label `preview` added by @zanieb on 2024-06-25 00:37_

---

_Label `preview` added by @zanieb on 2024-06-25 00:37_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-25 01:22_

---

_Review requested from @konstin by @zanieb on 2024-06-25 01:22_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:264 on 2024-06-25 01:24_

This guy could probably move into `install-wheel-rs`

---

_@zanieb reviewed on 2024-06-25 01:24_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1896 on 2024-06-25 13:04_

Does this uses the venv or the base interpreter for the venv? For tool installation, we should try to pick a long-living python interpreter, i had cases where pipx tools broke because i uninstalled the interpreter i installed them with.

---

_Review comment by @konstin on `crates/uv-tool/src/lib.rs`:123 on 2024-06-25 13:06_

Hm?

---

_Review comment by @konstin on `crates/uv-tool/src/lib.rs`:155 on 2024-06-25 13:06_

Good call!

---

_Review comment by @konstin on `crates/uv/src/commands/tool/install.rs`:33 on 2024-06-25 13:14_

What's with this argument?

---

_@konstin approved on 2024-06-25 13:22_

A single `tools.toml` for all tools sounds like a way that could break all installed tool by corrupting a single file, what e.g. about `$UV_STATE_DIR/tools/$name/tool.toml` for each tool?

Should we add versioning to the toml file(s) to allow breaking changes in the format?

We should add locking to prevent concurrent modification of both tools and toml files; we already have an abstraction for this with `LockedFile::acquire(tool_dir.join(".lock"), tool_dir.user_display())`.

---

_@zanieb reviewed on 2024-06-25 13:28_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:123 on 2024-06-25 13:28_

Yeah this copy pasta is no good. Removed in a later commit but 'll drop here too.

---

_@zanieb reviewed on 2024-06-25 13:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:33 on 2024-06-25 13:29_

Copied over from `tool/run.rs` — I'm not sure.

---

_@zanieb reviewed on 2024-06-25 13:29_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1896 on 2024-06-25 13:29_

We ignore virtual environments during discovery. I'll update this documentation.

---

_Comment by @zanieb on 2024-06-25 13:31_

> A single tools.toml for all tools sounds like a way that could break all installed tool by corrupting a single file, what e.g. about $UV_STATE_DIR/tools/$name/tool.toml for each tool?

Seems reasonable, though I wanted a central file for bulk operations.  I guess we can read every directory instead pretty easily. It won't break the tools though it'd just stop you from installing more.

> Should we add versioning to the toml file(s) to allow breaking changes in the format?

Perhaps eventually, but it doesn't seem critical right now. Do you think we need to immediately?

> We should add locking to prevent concurrent modification of both tools and toml files; we already have an abstraction for this with LockedFile::acquire(tool_dir.join(".lock"), tool_dir.user_display()).

Sounds good to me.

---

_@zanieb reviewed on 2024-06-25 13:37_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:140 on 2024-06-25 13:37_

You're not a cache directory...

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:295 on 2024-06-25 13:37_

Will drop

---

_@zanieb reviewed on 2024-06-25 13:38_

---

_Comment by @konstin on 2024-06-25 13:40_

> Perhaps eventually, but it doesn't seem critical right now. Do you think we need to immediately?

I'd put a `version = 1` on top, if we never use it it's ~free and if we bump the version later all old versions error immediately.

---

_Comment by @zanieb on 2024-06-25 14:07_

I'm hesitating on multiple `tool` files. I eventually want people to be able to define their tools in a centralized file rather than in invocations.

---

_Comment by @zanieb on 2024-06-25 14:14_

I'm going to skip adding a version in this pull request, but I'm happy to do it next. I just want to think more carefully about how it works.

---

_Marked ready for review by @zanieb on 2024-06-25 14:20_

---

_@charliermarsh approved on 2024-06-26 01:19_

Looks good! My only concern here is around `tools.toml`, because it feels under-explored right now and (as far as I can tell) is unused at the moment. Will we always keep it in sync? What are the motivating use-cases?


---

_Merged by @zanieb on 2024-06-26 15:24_

---

_Closed by @zanieb on 2024-06-26 15:24_

---

_Branch deleted on 2024-06-26 15:24_

---

_Comment by @charliermarsh on 2024-06-26 15:25_

Rock on

---
