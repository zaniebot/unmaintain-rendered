```yaml
number: 7934
title: Validate that discovered interpreters meet the Python preference
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: release/080
head: zb/python-preference-filter
created_at: 2024-10-04T21:11:17Z
updated_at: 2025-07-16T20:31:49Z
url: https://github.com/astral-sh/uv/pull/7934
synced_at: 2026-01-10T06:53:01Z
```

# Validate that discovered interpreters meet the Python preference

---

_Pull request opened by @zanieb on 2024-10-04 21:11_

Closes https://github.com/astral-sh/uv/issues/5144

e.g.

```
❯ cargo run -q -- sync --python-preference only-system
Using CPython 3.12.6 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 9 packages in 14ms
Installed 8 packages in 9ms
 + anyio==4.6.0
 + certifi==2024.8.30
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.2
 + idna==3.10
 + ruff==0.6.7
 + sniffio==1.3.1
 
❯ cargo run -q -- sync --python-preference only-managed
Using CPython 3.12.1
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 9 packages in 14ms
Installed 8 packages in 11ms
 + anyio==4.6.0
 + certifi==2024.8.30
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.2
 + idna==3.10
 + ruff==0.6.7
 + sniffio==1.3.1
```

---

_Label `bug` added by @zanieb on 2024-10-04 21:11_

---

_@zanieb reviewed on 2024-10-04 21:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_tree.rs`:333 on 2024-10-04 21:37_

These tests broke for some reason — I don't really understand why but they're written incorrectly.

---

_@zanieb reviewed on 2024-10-04 21:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:672 on 2024-10-04 21:38_

This split into the function above for readability.

---

_Review requested from @konstin by @zanieb on 2024-10-07 21:38_

---

_@zanieb reviewed on 2024-10-07 21:39_

---

_Review comment by @zanieb on `crates/uv/tests/pip_tree.rs`:333 on 2024-10-07 21:39_

(This was a bug that I fixed in a subsequent commit)

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-08 14:46_

---

_Comment by @zanieb on 2024-10-08 15:32_

We should probably opt active virtual environments out of this?

---

_Comment by @konstin on 2024-10-08 15:36_

> We should probably opt active virtual environments out of this?

If i specifically pass `--python-preference only-system` i'd expect uv to invalidate the current venv and recreate, i sees this mismatch like requesting a different Python version than the currently active one. (Admittedly, this is a somewhat fringe problem)

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:296 on 2024-10-08 15:48_

Should we cache this? It's still faster than it needs to be, but it's still an io operation, so if we optimize this for speed, I'd cache this `read_dir` first.

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:605 on 2024-10-08 15:50_

nit: `filter_ok` to remove the indirection with the `result_` methods

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:700 on 2024-10-08 15:53_

Docstring from different function.

Could you add another sentence as to why this function exists, i.e., which case it prevents?

---

_@konstin approved on 2024-10-08 15:55_

---

_Comment by @zanieb on 2024-10-08 15:57_

> If i specifically pass --python-preference only-system i'd expect uv to invalidate the current venv and recreate,

But.. `uv run` outside a project and `uv pip install` can't just invalidate and recreate an environment — should they fail?

---

_Comment by @konstin on 2024-10-08 15:59_

I think so

---

_Removed from milestone `v0.5.0` by @zanieb on 2024-11-25 20:24_

---

_Added to milestone `v0.6.0` by @zanieb on 2024-11-25 20:24_

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Comment by @zanieb on 2025-04-30 17:37_

This just needs test coverage, which is in WIP locally.

---

_Removed from milestone `v0.7.0` by @zanieb on 2025-04-30 17:37_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-30 17:37_

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:883 on 2025-07-16 18:42_

nit: This can be a method on `PythonSource`

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 19:03_

How is `VIRTUAL_ENV` different here, doesn't that imply that I get different behavior depending on whether I activate the venv first and then `uv run` or whether I only use `uv run`, which is usually the same?

---

_@konstin reviewed on 2025-07-16 19:04_

---

_@zanieb reviewed on 2025-07-16 19:09_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 19:09_

`VIRTUAL_ENV` is set to the default virtual environment path (just by the test context)

This does imply you get different behavior based on whether the environment is active. I think that's correct though? With an active environment, it would be incorrect to ignore it (at most, we should warn), but when performing environment discovery, it makes sense to bypass environments that do not meet the preference.

---

_@konstin reviewed on 2025-07-16 19:36_

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 19:36_

If you have a project with `requires-python = ">=3.11"` and try a too old venv with

```
uv venv -p 3.9
uv run python --version
```

then uv will recreate the venv in a compatible version and you'll get e.g. `Python 3.13.5`. This is independent of whether the venv is activated or not. I'd be surprised if there was different behavior between the Python version and the Python source wrt to the when we invalidate.

More concretely, I'd expect both blocks to share the invalidation behavior:

```
uv run --python-preference only-system -p 3.10 python --version
uv run --python-preference only-system -p 3.11 python --version
```

```
uv run --python-preference only-system -p 3.10 python --version
uv run --python-preference managed -p 3.10 python --version
```

---

_Comment by @konstin on 2025-07-16 19:42_

This branch seems to have a bad interaction with the minor version links for Python upgrades (CC @jtfmumm). Rerunning the last command recreates the venv in a loop on my machine 

```
uv python uninstall --all --preview
uv init -p 3.10 foo
cd foo
uv run --preview --python-preference only-managed -p 3.10 python --version
```

Logs: https://gist.github.com/konstin/709fb6c4467fc607cfbea985f5ee8822

---

_@zanieb reviewed on 2025-07-16 19:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 19:53_

Yeah this behavior differs outside of projects because:

1. Outside of a project, we can't "invalidate" and recreate a virtual environment.
2. Projects don't consider `VIRTUAL_ENV` at all by default

This is intended to avoid making the breaking change too disruptive. While I might agree with you that it's better to have a consistent behavior, I think it's too risky to change.

---

_Comment by @zanieb on 2025-07-16 20:01_

That should be fixed by https://github.com/astral-sh/uv/pull/7934/commits/fbebb7ad603d743391c4fce3ee242bf9a49ef266

---

_@zanieb reviewed on 2025-07-16 20:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 20:03_

To be clear, you're commenting on a test that is covering the behavior of `uv run` outside of a project. For project environments, we have the behavior you're looking for.

---

_@konstin reviewed on 2025-07-16 20:11_

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:5529 on 2025-07-16 20:11_

Oh, I see!

---

_@konstin approved on 2025-07-16 20:14_

---

_Merged by @zanieb on 2025-07-16 20:31_

---

_Closed by @zanieb on 2025-07-16 20:31_

---

_Branch deleted on 2025-07-16 20:31_

---
