```yaml
number: 4950
title: "Add `uv python pin`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/python-pin
created_at: 2024-07-09T23:18:02Z
updated_at: 2024-07-10T16:52:25Z
url: https://github.com/astral-sh/uv/pull/4950
synced_at: 2026-01-10T13:42:52Z
```

# Add `uv python pin`

---

_Pull request opened by @zanieb on 2024-07-09 23:18_

Adds a `uv python pin` command to write to a `.python-version` file.

We support all of our Python version request formats. We also support a `--resolved` flag to pin to a specific interpreter instead of the provided version. We canonicalize the request with #4949, it's not just printed verbatim. We always attempt to find the interpreter so we can warn if it's not available. With `--resolved`, if we can't find the interpreter we fail. If no arguments are provided, we'll attempt to display the current pin.

In the future:

- We should confirm that this satisfies the `Requires-Python` metadata if a `pyproject.toml` is present
- We should support writing to a `uv.python-version` field if `pyproject.toml` or `uv.toml` are present
- We should support finding and updating the "nearest" Python version file (looking in ancestors)
- We should support finding version files in workspaces
- We should support some sort of global pin



---

_Label `cli` added by @zanieb on 2024-07-09 23:18_

---

_Label `preview` added by @zanieb on 2024-07-09 23:18_

---

_Renamed from "zb/python pin" to "Add `uv python pin`" by @zanieb on 2024-07-09 23:18_

---

_Marked ready for review by @zanieb on 2024-07-09 23:32_

---

_Comment by @charliermarsh on 2024-07-10 00:28_

Is `.python-version` parsed by other tools, though?

---

_Comment by @zanieb on 2024-07-10 01:05_

I believe some tools will parse it, yes, but I'm not sure which. For most uses, we'll be writing a simple version. It may be fair to say if you go outside that it'll only be supported by us. We could also consider only supporting writing simple versions until we have `pyproject.toml` support, but we already support reading it so... idk.

---

_Comment by @zanieb on 2024-07-10 01:05_

I'll do some research, but this might just be a caveat in the documentation?

---

_Comment by @zanieb on 2024-07-10 01:12_

A brief Google search makes it look like just [pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#understanding-python-version-selection).

---

_Comment by @charliermarsh on 2024-07-10 02:00_

Mise also parses it and Rye does too IIRC.

---

_Comment by @zanieb on 2024-07-10 02:37_

So what are you thinking? What does mise do if it's not in the format it expects?

---

_Comment by @charliermarsh on 2024-07-10 02:50_

I'm not certain, I can test it out.

---

_Comment by @charliermarsh on 2024-07-10 02:57_

Actually, it looks like mise uses `.tool-versions` which is generic (not just for Python).

---

_Comment by @charliermarsh on 2024-07-10 03:00_

It looks like if Rye doesn't recognize it, it just gets ignored.

---

_Comment by @charliermarsh on 2024-07-10 03:01_

It just uses this:
```rust
/// Return the [`PythonVersionRequest`] from a `.python-version` file.
fn read_python_version(contents: &str) -> Option<PythonVersionRequest> {
    // Skip empty lines and comments.
    let ver = contents.lines().find(|line| {
        let trimmed = line.trim();
        !(trimmed.is_empty() || trimmed.starts_with('#'))
    })?;

    // Parse the version.
    let ver = ver.parse().ok()?;

    Some(ver)
}
```

---

_Comment by @zanieb on 2024-07-10 06:00_

Thanks for checking!

I think I'd prefer to support the full syntax of Python requests we support (e.g., it makes sense to allow pinning to `pypy@3.12` if that's what your project needs) and just clearly document that other tools may not support those complex requests? We could prioritize `pyproject.toml` support and only allow the extended syntax there but then people who aren't working in "projects" don't have a way to pin local Python versions consistent with the rest of our interface.

---

_@charliermarsh reviewed on 2024-07-10 15:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:80 on 2024-07-10 15:09_

Nit: trailing commas here

---

_@charliermarsh approved on 2024-07-10 15:10_

---

_@zanieb reviewed on 2024-07-10 15:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:80 on 2024-07-10 15:17_

Noo!

---

_Merged by @zanieb on 2024-07-10 16:52_

---

_Closed by @zanieb on 2024-07-10 16:52_

---

_Branch deleted on 2024-07-10 16:52_

---
