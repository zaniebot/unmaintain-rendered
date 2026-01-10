```yaml
number: 11667
title: Display path separators as backslashes on Windows
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/windows-path-separators
created_at: 2025-02-20T14:42:20Z
updated_at: 2025-02-28T09:28:18Z
url: https://github.com/astral-sh/uv/pull/11667
synced_at: 2026-01-10T11:10:38Z
```

# Display path separators as backslashes on Windows

---

_Pull request opened by @jtfmumm on 2025-02-20 14:42_

Currently, `uv tool list --show-paths` will show backslashes as path separators for packages but not entrypoints. This PR changes this to be consistent.

Closes #10426.


---

_Comment by @zanieb on 2025-02-20 16:17_

What's the root cause of this? Is this because they're stored with Unix-style paths in the tool receipt?

https://github.com/astral-sh/uv/blob/c30f53b295130408adba0054a00e598b93add2dc/crates/uv-tool/src/tool.rs#L289-L293

Should we just deserialize these into the appropriate platform-specific type? Or perhaps we should implement `Display` for `ToolEntrypoint`?

Otherwise, I feel like we'll just be playing whack-a-mole with places these are displayed.

---

_Comment by @jtfmumm on 2025-02-20 16:24_

> What's the root cause of this? Is this because they're stored with Unix-style paths in the tool receipt?
> 
> https://github.com/astral-sh/uv/blob/c30f53b295130408adba0054a00e598b93add2dc/crates/uv-tool/src/tool.rs#L289-L293
> 
> Should we just deserialize these into the appropriate platform-specific type? Or perhaps we should implement `Display` for `ToolEntrypoint`?
> 
> Otherwise, I feel like we'll just be playing whack-a-mole with places these are displayed.

Yeah, implementing `Display` on `ToolEntrypoint` might be the right choice here.

---

_@zanieb reviewed on 2025-02-20 19:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:254 on 2025-02-20 19:27_

We should use this in `new_with_versions` too

---

_@jtfmumm reviewed on 2025-02-20 19:33_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:254 on 2025-02-20 19:33_

You mean remove the existing temp dir filter from `new_with_versions` and close the method with:
```
        let context = Self {
            root: ChildPath::new(root.path()),
            temp_dir,
            cache_dir,
            python_dir,
            home_dir,
            bin_dir,
            venv,
            workspace_root,
            python_version,
            python_versions,
            filters,
            extra_env: vec![],
            _root: root,
        };
        context.with_filtered_temp_dir()
```
?

---

_@zanieb reviewed on 2025-02-20 19:39_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:254 on 2025-02-20 19:39_

I did mean that, but hm, that could break the ordering.

We might need to move all of the filters out into utilities like this so we can do so without changing our default ordering. I think it's fine to pursue separately — can you open an issue to track? No need to block this pull request on it.

---

_@zanieb approved on 2025-02-20 19:40_

---

_@jtfmumm reviewed on 2025-02-20 20:14_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:254 on 2025-02-20 20:14_

Turns out this needs to be specialized for Windows. I've updated and renamed to `with_filtered_windows_temp_dir()`

---

_Marked ready for review by @jtfmumm on 2025-02-20 20:54_

---

_Merged by @jtfmumm on 2025-02-20 21:28_

---

_Closed by @jtfmumm on 2025-02-20 21:28_

---

_Branch deleted on 2025-02-20 21:28_

---

_Label `bug` added by @jtfmumm on 2025-02-28 09:28_

---
