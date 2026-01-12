```yaml
number: 6711
title: Fix infinite recursion by sniffing the shebang
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
base: main
head: konsti/fix-infinite-recursion-by-sniffing-the-shebang
created_at: 2024-08-27T18:10:45Z
updated_at: 2024-10-17T09:46:30Z
url: https://github.com/astral-sh/uv/pull/6711
synced_at: 2026-01-12T16:07:29Z
```

# Fix infinite recursion by sniffing the shebang

---

_@konstin_

When we see `#!/usr/bin/env -S uv run"`, treat the input as a python file to avoid infinite recursion.

Tested manually.

Fixes #6360

---

_Label `bug` added by @konstin on 2024-08-27 18:10_

---

_Review requested from @charliermarsh by @konstin on 2024-08-27 18:10_

---

_Review requested from @zanieb by @konstin on 2024-08-27 18:10_

---

_@teroshan reviewed on 2024-08-28 13:10_

---

_Review comment by @teroshan on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 13:10_

This wouldn't take into account for cases where e.g. `env -S` isn't supported (older version of coreutils), where some black magic or custom script (e.g. named `uv-run` in my case) is used in the shebang.

Also it wouldn't be completely alien to want to hardcode a path to `uv` instead of relying to `env`.

In both of these cases the shebang wouldn't match, and this heuristic would not completely fix the infinite recursion issue.

---

_@zanieb reviewed on 2024-08-28 13:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 13:36_

I also worry this heuristic is too brittle. I'm not sure what the solution is without peeking. Some sort of environment variable?

---

_@teroshan reviewed on 2024-08-28 13:47_

---

_Review comment by @teroshan on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 13:47_

My two cents as I'm still discovering and playing with `uv`, I'd be happy with an alternative entry point (e.g. `uv exec`) which would only accept files as input.

It seems to me that this whole issue is made complex by `uv run` also being expected to run 'scripts' declared in `pyproject.toml` (I think?), and being bent to also work like a standalone script runner.

EDIT: I feel like a CLI flag to force a file to be executed would also be acceptable.

---

_@zanieb reviewed on 2024-08-28 13:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 13:53_

We also have something like... #3097 — maybe we just need a `--script` flag here to force us to treat something as a script? We should still have a way to detect recursion and bail instead of looping though.

---

_@zanieb reviewed on 2024-08-28 13:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 13:54_

(i.e. a `UV_INTERNAL__RECURSION` variable that we bump on every recursive subprocess and bail after some limit is hit)

---

_@konstin reviewed on 2024-08-28 14:38_

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 14:38_

A `--script` flag plus a `UV_INTERNAL_RECURSION` for better error reporting would work. 

---

_@konstin reviewed on 2024-08-28 14:39_

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 14:39_

A more resilient solution would be not running binaries, but only running python scripts and `python` from `uv run`, this seems to be done by e.g. `bash` to avoid infinite recursion.

---

_@zanieb reviewed on 2024-08-28 14:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:887 on 2024-08-28 14:40_

I'm pretty unwilling to change that part of the user experience — running binaries from `uv run` is important. I think a separate command or opt-in to that experience via `--script` makes the most sense to me.

---

_Closed by @konstin on 2024-10-17 09:46_

---
