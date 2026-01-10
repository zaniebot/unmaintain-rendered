```yaml
number: 4791
title: "Implement `uv init`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/uv-init
created_at: 2024-07-03T22:19:56Z
updated_at: 2024-07-19T15:11:49Z
url: https://github.com/astral-sh/uv/pull/4791
synced_at: 2026-01-10T13:42:52Z
```

# Implement `uv init`

---

_Pull request opened by @ibraheemdev on 2024-07-03 22:19_

## Summary

Implements the `uv init` command, which initializes a project (`pyproject.toml`, `README.md`, `src/__init__.py`) in the current directory, or in the given path. `uv init` also does workspace discovery.

Resolves https://github.com/astral-sh/uv/issues/1360.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-03 22:20_

---

_Review requested from @zanieb by @ibraheemdev on 2024-07-03 22:20_

---

_Label `preview` added by @ibraheemdev on 2024-07-03 22:20_

---

_@ibraheemdev reviewed on 2024-07-03 22:20_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-03 22:20_

Anything else we should add here? I think Rye and Poetry also fill in the authors field, probably through git config?

---

_@ibraheemdev reviewed on 2024-07-03 22:21_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-03 22:21_

`uv init` also doesn't currently run `git init` or support `--vcs`, which cargo does.

---

_@zanieb reviewed on 2024-07-03 23:23_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1666 on 2024-07-03 23:23_

I'm not sure we want to create this file. We need to support pinning via the `pyproject.toml` too.

---

_@zanieb reviewed on 2024-07-03 23:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:24 on 2024-07-03 23:24_

I thought we dropped this lint

---

_@zanieb reviewed on 2024-07-03 23:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:55 on 2024-07-03 23:25_

What if we're given an absolute path?

---

_@ibraheemdev reviewed on 2024-07-04 00:02_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/init.rs`:55 on 2024-07-04 00:02_

Fixed.

---

_Comment by @zanieb on 2024-07-04 00:43_

Sorry I only got part way through reviewing and had to go — I'll try to give it another look soon.

---

_@imbev reviewed on 2024-07-05 21:22_

---

_Review comment by @imbev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-05 21:22_

> Anything else we should add here? I think Rye and Poetry also fill in the authors field, probably through git config?

The command could prompt, using the git config or example, example@example.com as a default.

---

_@imbev reviewed on 2024-07-05 21:24_

---

_Review comment by @imbev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-05 21:24_

I'm not sure if it's within the scope of this PR, but there could be a boolean prompt that could lead to further prompting to add package-specific fields such as keywords, classifiers, urls

---

_@bluss reviewed on 2024-07-14 10:55_

---

_Review comment by @bluss on `crates/uv/src/commands/project/init.rs`:85 on 2024-07-14 10:55_

Ideally only fill in the readme key if actually creating the README. (Rye has a nice goal that it creates a working configuration without extra steps.)

---

_Comment by @zanieb on 2024-07-15 16:20_

Let me know when this is ready for another look

---

_Converted to draft by @zanieb on 2024-07-15 16:21_

---

_@ibraheemdev reviewed on 2024-07-18 20:54_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-18 20:54_

The other thing missing is `requires-python`, should we default to `>=3.12`?

---

_Marked ready for review by @ibraheemdev on 2024-07-18 20:55_

---

_Review requested from @zanieb by @ibraheemdev on 2024-07-18 20:55_

---

_@imbev reviewed on 2024-07-18 20:56_

---

_Review comment by @imbev on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-18 20:56_

uv has a default python executable, perhaps requires-python could be set to the same version?

---

_@zanieb reviewed on 2024-07-18 21:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:89 on 2024-07-18 21:07_

Defaulting to the latest seems.. reasonable. 

---

_@zanieb approved on 2024-07-19 00:36_

Let's go :)

---

_@zanieb reviewed on 2024-07-19 14:25_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1666 on 2024-07-19 14:25_

ref #4970 

---

_Merged by @zanieb on 2024-07-19 15:11_

---

_Closed by @zanieb on 2024-07-19 15:11_

---

_Branch deleted on 2024-07-19 15:11_

---
