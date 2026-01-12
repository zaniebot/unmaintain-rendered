```yaml
number: 8458
title: "Install versioned Python executables into the bin directory during `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zb/python-exec-dir
created_at: 2024-10-22T14:23:19Z
updated_at: 2024-10-30T21:44:08Z
url: https://github.com/astral-sh/uv/pull/8458
synced_at: 2026-01-12T16:08:19Z
```

# Install versioned Python executables into the bin directory during `uv python install`

---

_@zanieb_

Updates `uv python install` to link `python3.x` in the executable directory (i.e., `~/.local/bin`) to the the managed interpreter path.

Includes

- #8569 
- #8571 

Remaining work

- #8663 
- #8650 
- Add an opt-out setting and flag
- Update documentation

---

_Label `enhancement` added by @zanieb on 2024-10-22 14:23_

---

_Label `breaking` added by @zanieb on 2024-10-22 14:23_

---

_Renamed from "zb/python exec dir" to "Install versioned Python executables into the bin directory during `uv python install`" by @zanieb on 2024-10-22 14:23_

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-22 14:23_

---

_@zanieb reviewed on 2024-10-22 22:58_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:342 on 2024-10-22 22:58_

This moved up, so `executable` always returns the _existing_ executable for an installation and we ensure all the canonical names exist here.

---

_@zanieb reviewed on 2024-10-22 22:58_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:419 on 2024-10-22 22:58_

This refactored for future changes.

---

_Review requested from @charliermarsh by @zanieb on 2024-10-22 23:06_

---

_Marked ready for review by @zanieb on 2024-10-22 23:06_

---

_Comment by @zanieb on 2024-10-24 16:20_

Hm. Windows...

```
          5 │+error: Failed to create canonical Python executable at cpython-3.12.[X]-windows-x86_64-none/python.exe from cpython-3.12.[X]-windows-x86_64-none/python.exe
          6 │+  Caused by: failed to copy file from cpython-3.12.[X]-windows-x86_64-none/python.exe to cpython-3.12.[X]-windows-x86_64-none/python.exe
          7 │+  Caused by: The process cannot access the file because it is being used by another process. (os error 32)
```

Annoying this doesn't throw an already exists

---

_Comment by @charliermarsh on 2024-10-28 20:50_

How does this interact with your intent to add a versioned-binaries directory? (Is this the versioned-binaries directory? I assumed this was separate.)

---

_Review comment by @charliermarsh on `crates/uv-python/src/managed.rs`:344 on 2024-10-28 20:55_

Can we remove this when we update our bundled Pythons, given that we control the set of Pythons that would be installed?

---

_@charliermarsh reviewed on 2024-10-28 20:55_

---

_@charliermarsh reviewed on 2024-10-28 20:55_

---

_Review comment by @charliermarsh on `crates/uv-python/src/managed.rs`:344 on 2024-10-28 20:55_

(Missed that this was already present, anyway...)

---

_@charliermarsh reviewed on 2024-10-28 20:57_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:45 on 2024-10-28 20:57_

I might remove `into`, it's a trailing preposition

---

_@zanieb reviewed on 2024-10-28 20:57_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:344 on 2024-10-28 20:57_

I think so, once I fix it upstream. Note this is from #8310 

---

_@zanieb reviewed on 2024-10-28 20:58_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:45 on 2024-10-28 20:58_

Yeah I found this awkward both ways. Could I rephrase further somehow?

---

_Comment by @zanieb on 2024-10-28 21:02_

> How does this interact with your intent to add a versioned-binaries directory? (Is this the versioned-binaries directory? I assumed this was separate.)

Yes, sorry for the lack of clarity. The intent here is to provide multiple binaries on the `PATH` instead of a shim. Instead of placing these in some internal directory that we add to the `PATH`, we re-use our canonical location for installing executables. I think this provides a consistent experience with tool installation. And, since uv itself is moving here (#8420), installation should nearly always "just work" without further action.

There will probably be more work in the future to construct stable intermediary directories, e.g. solving for the problems raised in #8481 with an approach like Homebrew.

---

_Comment by @charliermarsh on 2024-10-28 21:14_

As a user, I'm conflicted on whether I would want this. I'd end up with a bunch of `python3.X` executables in `~/.local/bin`, without a clear connection to uv, and it would happen automatically, which some might argue is "polluting the PATH". How did you think about the tradeoffs between using `~/.local/bin`, and using a dedicated directory for uv pythons?

---

_Comment by @zanieb on 2024-10-28 21:25_

> I'd end up with a bunch of python3.X executables in ~/.local/bin, without a clear connection to uv, and it would happen automatically, which some might argue is "polluting the PATH". 

Perhaps there's some confusion around "it would happen automatically" — this is not the case. This should not happen during an implicit installation, e.g., `uv run -p 3.11 ...`, it should only happen during an explicit request to install, e.g., `uv python install 3.11`. I think this matches user expectations — a binary is placed on the `PATH` during a request to install Python. There will not be implicit pollution of the `PATH`.

> How did you think about the tradeoffs between using ~/.local/bin, and using a dedicated directory for uv pythons?

We'll only place versioned executables on the `PATH` without opt-in (i.e.,`--default` for an un-versioned Python), which I think reduces the cost of collisions. The alternative behavior we were considering was installation of a `python` shim executable in the `PATH` by default — I think this is significantly less problematic than that.

A separate directory for uv Python executables increases friction and is another thing we need to teach. My goal is to help beginners get going faster — I don't think power users have much difficulty getting Python on their `PATH`. I think removing the need to `source` or to otherwise update the shell is a big improvement in usability for beginners in particular. We'd also need to put it somewhere — probably in the `uv python dir`? I think installing executables in a consistent discoverable location instead is easier for users to understand.

There's also plenty of opportunity for users to configure this. They can set `UV_PYTHON_BIN_DIR` and avoid polluting their default bin directory. Additionally, I haven't mentioned this before, but I think we'll need to include an opt-out for this install behavior. I think that will further reduce downsides for power users that want to do something different.

---

_Comment by @charliermarsh on 2024-10-29 00:29_

> Perhaps there's some confusion around "it would happen automatically" — this is not the case. This should not happen during an implicit installation, e.g., `uv run -p 3.11` ..., it should only happen during an explicit request to install, e.g., uv python install 3.11. I think this matches user expectations — a binary is placed on the `PATH` during a request to install Python. There will not be implicit pollution of the `PATH`.

Just to clarify: this is what I assumed, there wasn't any confusion here. I may just be anchored in the way things work today, whereby I can `uv python install` without affecting any global state.

I think I need to do some reading / thinking before responding to the rest.

---

_Comment by @zanieb on 2024-10-29 01:39_

Ah okay thanks for clarifying. Part of where I'm coming from is that I feel it's sensible for `uv python install` and `uv tool install` to have the same effect on global state.

---

_@charliermarsh approved on 2024-10-29 20:05_

As per our conversation on Discord, I think it could make sense to gate this on preview.

---

_Label `breaking` removed by @zanieb on 2024-10-30 13:37_

---

_Label `preview` added by @zanieb on 2024-10-30 13:37_

---

_Comment by @zanieb on 2024-10-30 13:37_

Going to gate with preview and rebase onto main.

---

_Merged by @zanieb on 2024-10-30 14:13_

---

_Closed by @zanieb on 2024-10-30 14:13_

---

_Branch deleted on 2024-10-30 14:13_

---

_Comment by @zanieb on 2024-10-30 21:44_

Note this also gated to Unix pending #8663 

---
