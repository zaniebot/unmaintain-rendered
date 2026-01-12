```yaml
number: 4596
title: Improve toolchain and environment missing error messages
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/toolchain-errors
created_at: 2024-06-27T18:25:57Z
updated_at: 2024-06-28T15:17:00Z
url: https://github.com/astral-sh/uv/pull/4596
synced_at: 2026-01-12T16:06:20Z
```

# Improve toolchain and environment missing error messages

---

_@zanieb_

The journey here can be seen in:

- #4587 
- #4589 
- #4594 

I collapsed all the commits here because only the last one in the stack got us to a "correct" error message.

There are a few architectural changes:

- We have a dedicated `MissingEnvironment` and `EnvironmentNotFound` type for `PythonEnvironment::find` allowing different error messages when searching for environments
- `ToolchainNotFound` becomes a struct with the `ToolchainRequest` which greatly simplifies missing toolchain error formatting
- `ToolchainNotFound` tracks the `EnvironmentPreference` so it can accurately report the locations checked

The messages look like this now, instead of the bland (and often incorrect): "No Python interpreter found in system toolchains".

```
❯ cargo run -q -- pip sync requirements.txt
error: No virtual environment found
❯ UV_TEST_PYTHON_PATH="" cargo run -q -- pip sync requirements.txt --system
error: No system environment found
❯ UV_TEST_PYTHON_PATH="" cargo run -q -- pip sync requirements.txt --python 3.12
error: No virtual environment found for Python 3.12
❯ UV_TEST_PYTHON_PATH="" cargo run -q -- pip sync requirements.txt --python 3.12 --system
error: No system environment found for Python 3.12
❯ UV_TEST_PYTHON_PATH="" cargo run -q -- toolchain find 3.12 --preview
error: No toolchain found for Python 3.12 in system path
❯ UV_TEST_PYTHON_PATH="" cargo run -q -- pip compile requirements.in
error: No toolchain found in virtual environments or system path
```

I'd like to follow this with hints, suggesting creating an environment or using system in some cases.

---

_Label `error messages` added by @zanieb on 2024-06-27 18:25_

---

_@zanieb reviewed on 2024-06-27 18:33_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:120 on 2024-06-27 18:33_

The enum is now a struct which contains the `ToolchainRequest` instead of de-structuring the request here.

---

_@zanieb reviewed on 2024-06-27 18:34_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:136 on 2024-06-27 18:34_

We lose the ability to distinguish between these cases in error messages, but I don't think it's particularly critical.

---

_Marked ready for review by @zanieb on 2024-06-27 18:40_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-27 19:02_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-27 19:03_

---

_@charliermarsh reviewed on 2024-06-27 21:32_

---

_Review comment by @charliermarsh on `crates/uv/tests/venv.rs`:267 on 2024-06-27 21:32_

What are your thoughts on "toolchain" as user-facing copy vs. interpreter? I feel like interpreter is more familiar to folks but seeing that you proactively changed it here.

---

_@charliermarsh reviewed on 2024-06-27 21:34_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:136 on 2024-06-27 21:34_

By "these cases", do you mean file vs. directory vs. executable?

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:136 on 2024-06-27 21:36_

Specifically whether its a missing directory or a missing executable in the directory. We'll always raise the latter now.

---

_@zanieb reviewed on 2024-06-27 21:36_

---

_@charliermarsh reviewed on 2024-06-27 21:36_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:1502 on 2024-06-27 21:36_

Would this be "virtual environments or managed or system toolchains"? I feel like it should be Oxford comma'd but I know that's tedious...

---

_@zanieb reviewed on 2024-06-27 21:36_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:136 on 2024-06-27 21:36_

It would also be harder to encode things like "this exists but is not an executable" in the future. In practice, I don't think we were even using the variant right now.

---

_@charliermarsh approved on 2024-06-27 21:37_

So the basic idea here is that rather than creating a `ToolchainNotFound` variant, we base the error message on the request itself? Makes sense to me.

What `ToolchainRequest` was being returned such that I saw that "error: No Python interpreters found in system toolchains" message in my `ruff-lsp` PR?

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:267 on 2024-06-27 21:38_

I'm not sure, but I'd like to be consistent. We're going to talk about managed toolchains a lot going forward so I think we might want to move in that direction.

A nuance is that we look for an interpreter in order to discover a toolchain. I guess that leans towards using "interpreter" here.

---

_@zanieb reviewed on 2024-06-27 21:38_

---

_@zanieb reviewed on 2024-06-27 21:39_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:1502 on 2024-06-27 21:39_

😬 I know right. I considered doing an extra match to fix this.... but I didn't have it in me. Think it's worth it? I might want to construct a test covering this so I know it matters if I do it.

---

_Comment by @zanieb on 2024-06-27 21:43_

> So the basic idea here is that rather than creating a ToolchainNotFound variant, we base the error message on the request itself? Makes sense to me.

Yep, though that kind of just fell out of me making some other changes to actually propagate the information for correct error messages.

> What ToolchainRequest was being returned such that I saw that "error: No Python interpreters found in system toolchains" message in my ruff-lsp PR?

This was because the `ToolchainNotFound` didn't track `EnvironmentPreference` so when constructing the error message  we only had the `ToolchainPreference` and did something dumb. In your case, it was probably `ToolchainRequest::Any`, `EnvironmentPreference::ExplicitSystem`, and `ToolchainPreference::OnlySystem` (because we don't allow mutation of managed toolchains).

---

_@konstin approved on 2024-06-28 11:51_

---

_Review comment by @BurntSushi on `crates/uv/tests/venv.rs`:267 on 2024-06-28 12:08_

I weakly prefer interpreter I think. It seems likely to land better with Python users. But the inconsistency with using "toolchain" in written docs does seem unfortunate. How crazy would it be to switch to "interpreter" everywhere?

---

_@BurntSushi approved on 2024-06-28 12:09_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:267 on 2024-06-28 14:57_

Well.. I talked about that in the initial design and @konstin did clarify that there's much more than the interpreter involved e.g. the managed toolchains include the standard library.

---

_@zanieb reviewed on 2024-06-28 14:57_

---

_@zanieb reviewed on 2024-06-28 14:57_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:267 on 2024-06-28 14:57_

For this change, I'll use "interpreter" because I want to get these changes out asap.

---

_@BurntSushi reviewed on 2024-06-28 15:02_

---

_Review comment by @BurntSushi on `crates/uv/tests/venv.rs`:267 on 2024-06-28 15:02_

Ah yeah I hear ya. If y'all already dug into the naming here then I defer to you. :)

---

_@zanieb reviewed on 2024-06-28 15:09_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:267 on 2024-06-28 15:09_

I have yet to be convinced that "toolchain" is the best user-facing concept :) we should all chat more.

---

_Merged by @zanieb on 2024-06-28 15:16_

---

_Closed by @zanieb on 2024-06-28 15:16_

---

_Branch deleted on 2024-06-28 15:17_

---
