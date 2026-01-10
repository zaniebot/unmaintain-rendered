---
number: 6797
title: "`uv sync` does not install a workspace member's dependency with extra"
type: issue
state: closed
author: shunichironomura
labels:
  - question
assignees: []
created_at: 2024-08-29T09:04:44Z
updated_at: 2024-10-30T20:37:44Z
url: https://github.com/astral-sh/uv/issues/6797
synced_at: 2026-01-10T01:24:06Z
---

# `uv sync` does not install a workspace member's dependency with extra

---

_Issue opened by @shunichironomura on 2024-08-29 09:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Context

Consider the following project structure where:

- The root project is the workspace root with two members: `a-lib` and `an-app`
- The root project has `a-lib` as a dependency
- The `a-lib` library has an optional dependency `network = ["httpx>=0.27.2"]`
- The `an-app` application has a dependency `"a-lib[network]"`

```
.
├── README.md
├── a-lib
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── a_lib
│           └── __init__.py
├── an-app
│   ├── app.py
│   └── pyproject.toml
├── hello.py
├── pyproject.toml
└── uv.lock
```

## Expected behavior

- `uv sync` should install `httpx` as:
  - `httpx` is in the `network` extra of `a-lib`
  - `a-lib[network]` is a dependency of `an-app`, which is a workspace member

## Actual behavior

- Running `uv sync` in the repository root does not install `httpx`.

Output of `uv sync -v`:

```
DEBUG uv 0.4.0
DEBUG Found project root: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Found workspace root: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding current workspace member: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/a-lib`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/an-app`
DEBUG Searching for Python >=3.12 in managed installations or system path
DEBUG Searching for managed installations at `/Users/nomura/Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.12.5-macos-aarch64-none`
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/nomura/Library/Application Support/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3` (managed installations)
Using Python 3.12.5
Creating virtualenv at: .venv
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/nomura/Library/Caches/uv/built-wheels-v3/editable/927367c801095033`
DEBUG Found static `pyproject.toml` for: a-lib @ file:///Users/nomura/tmp/uv-opt-dep/a-lib
DEBUG Found workspace root: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding root workspace member: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding current workspace member: `/Users/nomura/tmp/uv-opt-dep/a-lib`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/an-app`
DEBUG Acquired lock for `/Users/nomura/Library/Caches/uv/built-wheels-v3/path/380ad038dfb54863`
DEBUG Found static `pyproject.toml` for: an-app @ file:///Users/nomura/tmp/uv-opt-dep/an-app
DEBUG Found workspace root: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding root workspace member: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding current workspace member: `/Users/nomura/tmp/uv-opt-dep/an-app`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/a-lib`
DEBUG Acquired lock for `/Users/nomura/Library/Caches/uv/built-wheels-v3/path/f80ace0e74a88082`
DEBUG Found static `pyproject.toml` for: uv-opt-dep @ file:///Users/nomura/tmp/uv-opt-dep
DEBUG Found workspace root: `/Users/nomura/tmp/uv-opt-dep/`
DEBUG Adding current workspace member: `/Users/nomura/tmp/uv-opt-dep/`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/a-lib`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/an-app`
DEBUG Acquired lock for `/Users/nomura/Library/Caches/uv/built-wheels-v3/editable/927367c801095033`
DEBUG Found static `pyproject.toml` for: a-lib @ file:///Users/nomura/tmp/uv-opt-dep/a-lib
DEBUG Found workspace root: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding root workspace member: `/Users/nomura/tmp/uv-opt-dep`
DEBUG Adding current workspace member: `/Users/nomura/tmp/uv-opt-dep/a-lib`
DEBUG Adding discovered workspace member: `/Users/nomura/tmp/uv-opt-dep/an-app`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 10 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Directory source requirement already cached: a-lib==0.1.0 (from file:///Users/nomura/tmp/uv-opt-dep/a-lib)
Installed 1 package in 0.78ms
 + a-lib==0.1.0 (from file:///Users/nomura/tmp/uv-opt-dep/a-lib)
```

- If you remove the `project.optional-dependencies` section from `a-lib/pyproject.toml` and run `uv add httpx --optional network`, it _will_ install `httpx` as expected. However, running `uv sync` will remove `httpx` again.



## For reproduction

For reproduction, please follow the instruction on this repository: https://github.com/shunichironomura/uv-opt-dep

## Environment

- uv platform: macOS
- uv version: `uv 0.4.0 (d9bd3bc7a 2024-08-28)`


---

_Renamed from "`uv sync` does not install a member's dependency with extra" to "`uv sync` does not install a workspace member's dependency with extra" by @shunichironomura on 2024-08-29 09:05_

---

_Comment by @charliermarsh on 2024-08-29 13:01_

Thanks for the thorough writeup and repro, I will read it closely and take a look today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-29 14:04_

---

_Comment by @charliermarsh on 2024-08-29 14:08_

I cloned the repo, but the root `pyproject.toml` was missing the extra:

```diff
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -4,7 +4,7 @@ version = "0.1.0"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.12"
-dependencies = ["a-lib"]
+dependencies = ["a-lib[network]"]

 [tool.uv.sources]
 a-lib = { workspace = true }
```

After that, it works as expected. Can you give it a try?

---

_Label `question` added by @charliermarsh on 2024-08-29 14:08_

---

_Comment by @shunichironomura on 2024-08-29 14:47_

Thank you for the comment, it worked. However, although adding the extra in the root `pyproject.toml` is a valid workaround, my intention was to reflect the actual requirements that the extra is only required by the `an-app`, and not required by the root project (i.e., the `hello.py` script in the above example).

Or maybe specifying different extras for one dependency across workspace members is invalid in the first place?

---

_Comment by @charliermarsh on 2024-08-29 14:51_

You may be looking for `uv sync --package an-app`? This would sync the dependencies of `an-app`, rather than the workspace root. (They use the same lockfile though.)

---

_Comment by @shunichironomura on 2024-08-29 15:09_

Now I realized that I had a wrong mental model of how `uv` workspace works. I assumed running `uv sync` will install the union of workspace members' dependencies into the virtualenv where you can work on all the workspace member packages at the same time. But you actually select which project's dependencies to install from the lock file, which is shared by the workspace members... Sorry for taking your time.

So in the development phase, do I need to run `uv sync (--package ...)` every time I change the workspace member to work on?

---

_Comment by @charliermarsh on 2024-08-29 15:26_

If you use `uv run` or `uv run --package`, uv will automatically sync prior to running the command (if you're out-of-sync). Alternatively, if you `uv run` or `uv sync` from _within_ the member directory (like, in `./an-app`), it will automatically sync that member.

---

_Comment by @shunichironomura on 2024-08-29 15:39_

Understood. Thank you!

---

_Closed by @shunichironomura on 2024-08-29 15:39_

---

_Comment by @vvuk on 2024-10-30 20:29_

Hmm -- this behaviour was pretty unexpected. I've got a workspace set up with a number of members that are plugins (comfyui custom node directories) to comfyui. I've got a pyproject.toml set up for each of them, but comfyui doesn't actually directly depend on them; they're discovered dynamically.

I'd really like `uv sync` in the workspace root to install all dependencies (basically, everything in the lock file) up front. Is there any reason to not do this? Otherwise I'd need to manually uv sync every single plugin. Worst case a flag like `uv sync --all`, but I'd think that should be the default in the workspace root and there should be a flag to limit to just the workspace root. (Rough-and-very-imperfect analogy to `cargo` -- a `cargo build` in the workspace root builds all projects; it's explicitly a workspace after all.)


---

_Comment by @charliermarsh on 2024-10-30 20:37_

@vvuk -- You might be interested in https://github.com/astral-sh/uv/issues/5727 -- probably better to discuss there than in this closed issue.

---
