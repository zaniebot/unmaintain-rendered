---
number: 17307
title: uv add results in relative path for a local package
type: issue
state: open
author: need47
labels:
  - bug
  - breaking
assignees: []
created_at: 2026-01-02T19:14:26Z
updated_at: 2026-01-06T04:28:48Z
url: https://github.com/astral-sh/uv/issues/17307
synced_at: 2026-01-10T01:26:16Z
---

# uv add results in relative path for a local package

---

_Issue opened by @need47 on 2026-01-02 19:14_

### Summary

When using uv to add a local package that is specified by absolute path:
```
uv add /path/to/local/package
```

It resulted in a relative path in pyproject.toml and uv.lock:
```
[tool.uv.sources]
pymore = { path = "../../../path/to/local/package"}
```

Is this expected? I think keeping the absolute path makes more sense

### Platform

Linux 64

### Version

uv 0.9.21

### Python version

_No response_

---

_Label `bug` added by @need47 on 2026-01-02 19:14_

---

_Comment by @nooscraft on 2026-01-04 04:58_

I looked into this and can confirm this is **currently expected behavior**, though I completely understand why it's surprising! 🙂

When you run `uv add /absolute/path/to/package`, uv intentionally converts it to a relative path. This happens in the code here:

**`crates/uv-workspace/src/pyproject.rs`**
```rust
path: PortablePathBuf::from(
    relative_to(&install_path, root)
        .or_else(|_| std::path::absolute(&install_path))
        // ...
),
```

The logic is try to make it relative first, only keep it absolute if that fails (like when paths are on different drives on Windows). The design uses PortablePathBuf for cross-platform path serialization - converting Windows backslashes to forward slashes and making paths relative to the project root for portability.

I think we have two options:
- Preserve user intent: If the user provides an absolute value, retain it. If it’s relative, keep it relative.
- Improve documentation: Clearly explain the current behavior and the reasoning behind it.

@zanieb fyi!

---

_Comment by @zanieb on 2026-01-04 16:12_

Hm yeah it does seem important to allow absolute paths here.

---

_Label `breaking` added by @zanieb on 2026-01-04 16:12_

---

_Referenced in [astral-sh/uv#17316](../../astral-sh/uv/pulls/17316.md) on 2026-01-04 18:46_

---

_Comment by @nooscraft on 2026-01-04 18:51_

@zanieb I have raised a PR with a fix based on my knowledge. Please take a look.

---

_Comment by @charliermarsh on 2026-01-04 18:53_

Hmm, why do we want to retain absolute paths here? Note that even if we change the `pyproject.toml`, they'll probably be converted to relative in the `uv.lock` unless we make a more holistic change.

---

_Comment by @nooscraft on 2026-01-04 19:02_

@charliermarsh good catch, I missed that the lock file generation still normalizes paths to relative. I’m not too sure about the best way to preserve absolute paths during lock file generation, so I’ll leave this in the hands of the experts.

---

_Comment by @zanieb on 2026-01-04 23:02_

I think we'd need to retain it in the lockfile too.

I think it's a fair use-case to be able to move around your local project while depending on another project on your system. I'm not really sure how we should go about exposing it though.

---

_Comment by @konstin on 2026-01-05 18:24_

I'm in favor of preserving absolute and relative paths as absolute and relative through all types and inputs and output files, (incrementally) doing the more holistic change.

---

_Comment by @nooscraft on 2026-01-06 04:28_

I have updated the PR with a few more changes. I was trying to implement a holistic solution. The change preserves absolute/relative paths throughout the entire pipeline, from user input through `pyproject.toml` to `uv.lock`. Both files now respect the user’s original path format.
@konstin @zanieb fyi!

---
