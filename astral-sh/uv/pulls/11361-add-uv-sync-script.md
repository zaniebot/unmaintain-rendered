```yaml
number: 11361
title: "Add `uv sync --script`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/sync-dir
created_at: 2025-02-09T19:14:24Z
updated_at: 2025-02-12T16:02:19Z
url: https://github.com/astral-sh/uv/pull/11361
synced_at: 2026-01-12T16:09:48Z
```

# Add `uv sync --script`

---

_@charliermarsh_

## Summary

The environment is located at a stable path within the cache, based on the script's absolute path.

If a lockfile exists for the script, then we use our standard lockfile semantics (i.e., update the lockfile if necessary, etc.); if not, we just do a `uv pip sync` (roughly).

Example usage:

```
❯ uv init --script hello.py
Initialized script at `hello.py`

❯ uv add --script hello.py requests
Updated `hello.py`

❯ cargo run sync --script hello.py
Using script environment at: /Users/crmarsh/.cache/uv/environments-v1/hello-84e289fe3f6241a0
Resolved 5 packages in 3ms
Installed 5 packages in 12ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.3.0
```

Closes https://github.com/astral-sh/uv/issues/6637.


---

_Label `enhancement` added by @charliermarsh on 2025-02-09 19:15_

---

_Label `no-build` added by @charliermarsh on 2025-02-09 19:32_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-10 11:05_

Does it make sense to allow active e.g. for `uv sync --script --active scripts/publish/test_publish.py`? My use case is getting IDE support without explicit uv IDE support, having a shortcut for `uv export --script scripts/publish/test_publish.py | uv pip install -`. (I remember we had a discussion about that, but i can't find it)

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:140 on 2025-02-10 11:20_

`uv sync --group foo --script scripts/publish/test_publish.py` passes without warnings, is that intended? Same for `--extra`, both are banned by `uv run`.

With syncing support there's new a cause for groups in scripts (say a lint group that requires ruff because you want to format and lint the script), but i assume that discussion is for later.

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:6710 on 2025-02-10 11:29_

Should we add this filter to the test context, given that it's an unstable part of stderr for this workflow?

---

_@konstin reviewed on 2025-02-10 11:30_

---

_@charliermarsh reviewed on 2025-02-10 14:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:140 on 2025-02-10 14:49_

They should be banned. Groups and extras are not supported in scripts — they’re not in the standard.

---

_@charliermarsh reviewed on 2025-02-11 02:19_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-11 02:19_

I sort of defer to @zanieb here because it _could_ cause complications if we ever want to allow "run a script in the context of a project".

---

_Review requested from @zanieb by @charliermarsh on 2025-02-11 02:25_

---

_@zanieb reviewed on 2025-02-11 14:20_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-11 14:20_

> "run a script in the context of a project".

Can you describe what this means briefly? Layered on top of the project environment?

I think `--active` makes sense here. I presume it's awkward to support given how I implemented `--active` though?

---

_Comment by @zanieb on 2025-02-11 14:22_

> Perhaps controversially, running uv sync --script does create a lockfile for the script, if it doesn't exist. We can avoid that... it's just more work and more code.

It's pretty surprising that it does this, given it's otherwise opt-in. I'm hesitant to ask you to do more work, but...

Is there a reason it's that way — other than it being the easier implementation?

---

_Comment by @charliermarsh on 2025-02-11 14:23_

I can change it, it will just be a lot more code, since it requires `sync` to support these alternative semantics.

---

_Comment by @charliermarsh on 2025-02-11 14:23_

It's not a lot of _work_, it's just a lot more code (plus some test coverage).

---

_Comment by @zanieb on 2025-02-11 14:34_

Yeah that stinks... I think we should only change it if we want to take a firmer stance on scripts having lockfiles? The part that bothers me is that we'd create one on `uv sync` but not `uv run`. Should we create one during `uv run` too?

---

_Comment by @charliermarsh on 2025-02-11 16:42_

Sounds good -- I think I should just change it to be consistent with `uv run` for now.

---

_Comment by @charliermarsh on 2025-02-12 02:40_

Updated the PR to avoid creating a lockfile if it doesn't already exist.

---

_@charliermarsh reviewed on 2025-02-12 02:41_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-12 02:41_

I think `--active` wouldn't be too bad to support. I thought there was some thought that we might want you to be able to "run a script in the context of a project" -- I don't know what it means exactly, but there's some vague allusions to it in the code. Maybe, like, layering the script's dependencies atop the project's dependencies (like you said)?

---

_@charliermarsh reviewed on 2025-02-12 02:51_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-12 02:51_

Should we... warn, if `VIRTUAL_ENV` is set but the user didn't provide `--active` (following projects)?

---

_@charliermarsh reviewed on 2025-02-12 03:05_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3144 on 2025-02-12 03:05_

Did this separately: https://github.com/astral-sh/uv/pull/11433

---

_@zanieb reviewed on 2025-02-12 14:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3145 on 2025-02-12 14:28_

You know `conflicts_with_all` exists, right? :)

---

_@zanieb reviewed on 2025-02-12 14:30_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3139 on 2025-02-12 14:30_

Minor preference to keep this on one line for brevity / readability in the CLI short help.

---

_@zanieb approved on 2025-02-12 14:30_

---

_Merged by @charliermarsh on 2025-02-12 16:02_

---

_Closed by @charliermarsh on 2025-02-12 16:02_

---

_Branch deleted on 2025-02-12 16:02_

---
