```yaml
number: 5476
title: "Initialize a Git repository in `uv init`"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: init-vcs
created_at: 2024-07-26T12:12:10Z
updated_at: 2025-02-25T06:03:44Z
url: https://github.com/astral-sh/uv/pull/5476
synced_at: 2026-01-12T16:06:50Z
```

# Initialize a Git repository in `uv init`

---

_@j178_

## Summary

Similiar to `cargo init --vcs <VCS>`, this PR adds the `--vcs <VCS>` flag for `uv init`, allowing to create a version control system during initialization. By default, `uv init` will create a Git repository if the `--vcs` flag is not provided. Use `--vcs none` to disable this feature.

Currently, only Git is supported. While Cargo also supports hg, pijul, and fossil, this initial PR only includes Git. We can add more later if there are any user requests.


---

_Renamed from "`uv init` initializes a git repo" to "`uv init` initializes a Git repo" by @j178 on 2024-07-26 12:13_

---

_@j178 reviewed on 2024-07-26 12:20_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:368 on 2024-07-26 12:20_

This list of git ignores is taken from rye: https://github.com/astral-sh/rye/blob/main/rye/src/templates/gitignore.j2

---

_Marked ready for review by @j178 on 2024-07-27 09:04_

---

_@samypr100 reviewed on 2024-07-27 23:49_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/init.rs`:411 on 2024-07-27 23:49_

Might be worth using `which::which` (similar to interpreter search) to find `git` first and use its full bin path, and that also allows us to only run this when git is found in the path

---

_@zanieb reviewed on 2024-08-01 20:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:411 on 2024-08-01 20:23_

👍 agreed

---

_Assigned to @zanieb by @zanieb on 2024-08-01 20:23_

---

_Converted to draft by @j178 on 2024-08-07 12:52_

---

_@Ravencentric reviewed on 2024-09-04 15:45_

---

_Review comment by @Ravencentric on `crates/uv/src/commands/project/init.rs`:368 on 2024-09-04 15:45_

There's a more complete gitignore for python projects [here](https://github.com/github/gitignore/blob/main/Python.gitignore).

---

_@zanieb reviewed on 2024-09-04 15:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:368 on 2024-09-04 15:47_

(fwiw I find that way overbearing and would prefer to do a minimal gitignore)

---

_@Ravencentric reviewed on 2024-09-04 15:52_

---

_Review comment by @Ravencentric on `crates/uv/src/commands/project/init.rs`:368 on 2024-09-04 15:52_

I agree, especially the tool specific (pdm, poetry, pipfile, etc) ones can go, but I think atleast some from it could be hand picked and included by default, such as .mypy_cache

---

_Comment by @codspeed-hq[bot] on 2024-09-06 16:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/j178:init-vcs)

### Merging #5476 will **not alter performance**

<sub>Comparing <code>j178:init-vcs</code> (5861e4b) with <code>main</code> (4ba0e56)</sub>



### Summary

`✅ 14` untouched benchmarks






---

_Marked ready for review by @j178 on 2024-09-07 02:54_

---

_Renamed from "`uv init` initializes a Git repo" to "`uv init` initializes a git repo" by @j178 on 2024-09-08 07:31_

---

_Review requested from @zanieb by @j178 on 2024-09-08 07:34_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:368 on 2024-09-17 03:25_

I actually like the GitHub `.gitignore` and I always use it as a starter in my projects :D

---

_@charliermarsh reviewed on 2024-09-17 03:25_

---

_@zanieb reviewed on 2024-09-17 03:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:368 on 2024-09-17 03:52_

(And it drives me crazy, lol)

---

_@zanieb reviewed on 2024-09-17 03:54_

---

_Review comment by @zanieb on `crates/uv-configuration/src/vcs_options.rs`:55 on 2024-09-17 03:54_

It seems problematic not to capture and display the output of the command on failure. We have some utilities for this, I think.

---

_@zanieb reviewed on 2024-09-17 03:56_

---

_Review comment by @zanieb on `crates/uv-configuration/src/vcs_options.rs`:37 on 2024-09-17 03:56_

Does this mean if you don't have `git` installed and don't pass `--vcs git` we'll fail instead of just not using `git`? We might want to avoid that, either by returning a specific error variant here that the caller can ignore or by creating a separate case for the "default" behavior?

---

_@zanieb reviewed on 2024-09-17 03:57_

---

_Review comment by @zanieb on `crates/uv-configuration/src/vcs_options.rs`:44 on 2024-09-17 03:57_

Should we warn? It seems okay to just log this as `debug!`

---

_@zanieb reviewed on 2024-09-17 03:59_

---

_Review comment by @zanieb on `crates/uv-configuration/src/vcs_options.rs`:44 on 2024-09-17 03:59_

It looks like this is going to be handled by the caller with `existing_vcs_repo`?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:113 on 2024-09-17 04:00_

I feel like if you do `--vcs mercurial` and we're in a git repository we should fail instead of warning and ignoring?

---

_@zanieb reviewed on 2024-09-17 04:00_

---

_@zanieb reviewed on 2024-09-17 04:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:118 on 2024-09-17 04:01_

Should we be discarding _all_ errors here. For example, I think the `git` doesn't exist case makes sense to ignore, but not the `git init` command failed case. I'm not quite sure where the line is when a VCS isn't requested. Do you know what other tools do?

---

_@zanieb reviewed on 2024-09-17 04:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:118 on 2024-09-17 04:01_

(This may be a case for using `thiserror` or changing your API)

---

_@zanieb reviewed on 2024-09-17 04:03_

---

_Review comment by @zanieb on `crates/uv-configuration/src/vcs_options.rs`:37 on 2024-09-17 04:03_

I see this is handled downstream in a way quite similar to my note :) I added [a comment](https://github.com/astral-sh/uv/pull/5476/files#r1762250146) there.

---

_@charliermarsh approved on 2024-09-26 02:24_

Addressed @zanieb's feedback!

---

_Renamed from "`uv init` initializes a git repo" to "Initialize a Git repository in `uv init`" by @charliermarsh on 2024-09-26 02:34_

---

_Label `enhancement` added by @charliermarsh on 2024-09-26 02:34_

---

_Label `cli` added by @charliermarsh on 2024-09-26 02:34_

---

_Merged by @charliermarsh on 2024-09-26 02:40_

---

_Closed by @charliermarsh on 2024-09-26 02:40_

---

_Comment by @j178 on 2024-09-26 03:38_

Thanks @charliermarsh ❤️ 

---

_Branch deleted on 2024-09-26 03:52_

---

_Comment by @3f6a on 2025-02-25 06:03_

Is there a way to disable git repo creation by default globally?

---
