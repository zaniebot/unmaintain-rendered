```yaml
number: 12342
title: "Add `uv sync --check` flag"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: sync-check
created_at: 2025-03-20T16:27:32Z
updated_at: 2025-03-22T00:04:40Z
url: https://github.com/astral-sh/uv/pull/12342
synced_at: 2026-01-10T11:10:39Z
```

# Add `uv sync --check` flag

---

_Pull request opened by @blueraft on 2025-03-20 16:27_

## Summary

Closes #12338 

## Test Plan

`cargo test` 


---

_@zanieb reviewed on 2025-03-20 17:52_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3287 on 2025-03-20 17:52_

We should probably add another line saying like "uv will exit with a non-zero code if the environment is not up to date"

---

_@zanieb reviewed on 2025-03-20 17:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:373 on 2025-03-20 17:53_

Should this be exit code 2 or 1? I think we probably should use 1.

---

_@zanieb reviewed on 2025-03-20 17:55_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:383 on 2025-03-20 17:55_

I think we don't want `error: ` here, modeling after Ruff

```
❯ uvx ruff format --check
Installed 1 package in 2ms
51 files already formatted
❯ echo "import  bar" > test.py
❯ uvx ruff format --check
Would reformat: test.py
1 file would be reformatted, 51 files already formatted
❯ echo $?
1
```

---

_Label `enhancement` added by @zanieb on 2025-03-20 17:56_

---

_@blueraft reviewed on 2025-03-20 19:27_

---

_Review comment by @blueraft on `crates/uv/tests/it/sync.rs`:383 on 2025-03-20 19:27_

`uv sync --locked` already uses error: (exit code 2). Should we change both, or is it too risky due to potential breakage for users relying on the `--locked` exit code?

```
❯ uv sync --locked
Resolved 14 packages in 4ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

---

_@zanieb reviewed on 2025-03-20 20:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:383 on 2025-03-20 20:51_

Hm.. I'm not sure.

I think of the difference as: `--check` is asking uv to "check if my sync would do anything" whereas the other is asking uv to "sync but fail if the lockfile is outdated". In the first, it's not an error if sync would do something — that's the intent of the invocation. Whereas in the second, it's a failure to sync?

Unfortunately `uv lock --check` destroys my carefully constructed logic:

```
❯ uv lock --check
Using CPython 3.13.0
Resolved 4 packages in 86ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
❯ echo $?
2
```

I'd honestly be partial to changing that one specifically (in a separate PR), I think it's wrong. I think it's probably breaking to change though.

---

_@zanieb reviewed on 2025-03-20 20:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:383 on 2025-03-20 20:52_

I could be convinced to keep what you have here and change them both together in a future release.

---

_@zanieb reviewed on 2025-03-20 20:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:383 on 2025-03-20 20:52_

@charliermarsh ?

---

_@blueraft reviewed on 2025-03-21 14:43_

---

_Review comment by @blueraft on `crates/uv/tests/it/sync.rs`:383 on 2025-03-21 14:43_

Yeah makes sense, I'm okay with either option, lmk. Happy to make a follow up PR fixing that!

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3289 on 2025-03-21 15:28_

```suggestion
    /// If the environment is not up to date, uv will exit with an error.
```

---

_@zanieb reviewed on 2025-03-21 15:28_

---

_@zanieb reviewed on 2025-03-21 15:28_

---

_Review comment by @zanieb on `docs/reference/cli.md`:1518 on 2025-03-21 15:28_

```suggestion
<p>If the environment is not up to date, uv will exit with an error.</p>
```

---

_@zanieb reviewed on 2025-03-21 15:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:868 on 2025-03-21 15:29_

What do you think about something more like

```suggestion
    #[error("The environment is outdated; run `{}` to update the environment", "uv sync".cyan())]
```

---

_@zanieb reviewed on 2025-03-21 15:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:869 on 2025-03-21 15:30_

```suggestion
    OutdatedEnvironment,
```

---

_@zanieb reviewed on 2025-03-21 15:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:383 on 2025-03-21 15:30_

Let's stick with consistency to `uv lock` for now and open an issue to track a broader change.

---

_@zanieb approved on 2025-03-21 15:46_

---

_Merged by @zanieb on 2025-03-21 15:48_

---

_Closed by @zanieb on 2025-03-21 15:48_

---

_Comment by @zanieb on 2025-03-21 16:55_

If you're ever interested in some guidance on places we could use your help, feel free to reach out on Discord. Always appreciate your contributions.

---

_@charliermarsh reviewed on 2025-03-22 00:04_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:383 on 2025-03-22 00:04_

Yeah seems fine to ship this as-is and re-consider in a future release.

---
