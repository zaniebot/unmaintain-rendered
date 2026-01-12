```yaml
number: 7007
title: "`dev-dependencies` defined in `uv.toml` are not handled"
type: issue
state: closed
author: mkniewallner
labels:
  - documentation
assignees: []
created_at: 2024-09-04T11:37:59Z
updated_at: 2025-03-24T06:43:05Z
url: https://github.com/astral-sh/uv/issues/7007
synced_at: 2026-01-12T15:59:09Z
```

# `dev-dependencies` defined in `uv.toml` are not handled

---

_@mkniewallner_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

- uv version: 0.4.4
- OS: macOS 14.5

In theory, everything that lives under `[tool.uv]` section in `pyproject.toml` can also be defined in `uv.toml`. This includes [dev-dependencies](https://docs.astral.sh/uv/reference/settings/#dev-dependencies).

But given:
```toml
# pyproject.toml
[project]
name = "foo"
version = "0.0.1"
requires-python = ">=3.12"
```

```toml
# uv.toml
dev-dependencies = ["pytest==8.3.2"]
cache-dir = "/tmp"
```

When using `uv lock`, we end up with the following lock file:
```toml
version = 1
requires-python = ">=3.12"

[[package]]
name = "foo"
version = "0.0.1"
source = { virtual = "." }
```

so it doesn't seem that uv reads `dev-dependencies` from `uv.toml`.

But `uv.toml` itself is correctly read, as can be seen with:
```console
$ uv cache dir
/tmp
```

I realise that this is most likely on purpose, since it would be hard to handle dev dependencies in `uv.toml` (for instance `uv add --dev httpx` would currently add the dependency in `pyproject.toml`, even if we already have `dev-dependencies` in `uv.toml`. In that case, this most likely comes from the fact that the documentation is auto-generated based on the settings, and there's no way to mark some options as "only available in `pyproject.toml` when generating the documentation.

Note also than if also adding `dev-dependencies` in `pyproject.toml`, despite the warning, dev dependencies will still be handled properly from `pyproject.toml` when locking:
```toml
# pyproject.toml
# Adding the following section below.
[tool.uv]
dev-dependencies = ["mypy==1.11.2"]
```

```console
$ uv lock
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
Resolved 4 packages in 197ms
Added mypy v1.11.2
Added mypy-extensions v1.0.0
Added typing-extensions v4.12.2
```

Just in case, including the full logs from the very first `uv lock` command on the initial setup (without `[tool.uv]`), even if there's nothing really interesting:
```console
$ uv lock --verbose
DEBUG uv 0.4.4 (Homebrew 2024-09-04)
DEBUG Found workspace root: `/Users/mathieu.kniewallner/Workspaces/perso/renovate-uv-toml`
DEBUG Adding current workspace member: `/Users/mathieu.kniewallner/Workspaces/perso/renovate-uv-toml`
DEBUG The virtual environment's Python version satisfies `Python >=3.12`
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Acquired lock for `/tmp/built-wheels-v3/path/672fb4b165969c97`
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/mathieu.kniewallner/Workspaces/perso/renovate-uv-toml
DEBUG No workspace root found, using project root
DEBUG Released lock at `/tmp/built-wheels-v3/path/672fb4b165969c97/.lock`
DEBUG Solving with installed Python version: 3.12.1
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/mathieu.kniewallner/Workspaces/perso/renovate-uv-toml (*)
DEBUG Tried 1 versions: foo 1
DEBUG Split universal resolution took 0.001s
Resolved 1 package in 11ms
```

---

_Label `documentation` added by @charliermarsh on 2024-09-04 13:19_

---

_Comment by @charliermarsh on 2024-09-04 13:20_

I agree that we need to correct this is in the documentation... It's also true of `tool.uv.sources` and anything else that is considered project data rather than project configuration.

---

_Label `bug` added by @charliermarsh on 2024-09-04 13:20_

---

_Comment by @charliermarsh on 2024-09-04 13:44_

(I'm marking as `bug` because it's a bug in the docs arguably.)

---

_Comment by @mkniewallner on 2024-09-04 22:20_

Trying something out in https://github.com/astral-sh/uv/pull/7053, with the idea of separating the "project metadata" and "configuration" options into 2 distinct sections. WIP for now, as it needs a bit of polishing.

We would probably also need to:
- update `uv.schema.json` logic and how it's integrated into [schemastore](https://github.com/SchemaStore/schemastore), since currently it suffers from the same issue as the documentation, implying that everything we can set in `pyproject.toml` can also be set in `uv.toml` (maybe we'll unfortunately need to have 2 different files to ease the integration into `schemastore`?)
- update the warning shown when having both `uv.toml` and `pyproject.toml`, as technically it only is problematic if setting "configuration" options in `pyproject.toml` (right now it is also raised when using `uv.toml` but only setting "project metadata" options (like `dev-dependencies`) in `pyproject.toml`)

---

_Comment by @charliermarsh on 2024-09-14 22:12_

Do you think we should close this @mkniewallner?

---

_Comment by @mkniewallner on 2024-09-15 08:43_

> Do you think we should close this @mkniewallner?

I've noted 2 follow-up items that could further help clarify things in https://github.com/astral-sh/uv/issues/7007#issuecomment-2330252383, but they are probably lower priority than the documentation. If those are items you think are worth doing, maybe we could either rename this issue to make things more clear on what this is about, or create separate issue(s) for the remaining items? I'm personally fine with both, I just think it would be nice to keep a trace of the remaining issues, as I think fixing them at some point could improve user experience.

---

_Label `bug` removed by @charliermarsh on 2024-10-14 23:29_

---

_Comment by @charliermarsh on 2024-11-20 00:28_

We now error if `dev-dependencies` is specified in `uv.toml`. I think this is probably the best we can do for now without two separate schemas, and I don't know if two separate schemas is totally worth it. What do you think?

---

_Comment by @mkniewallner on 2024-11-20 08:22_

> We now error if `dev-dependencies` is specified in `uv.toml`. I think this is probably the best we can do for now without two separate schemas, and I don't know if two separate schemas is totally worth it. What do you think?

Yeah I believe we can close this issue, the most important item to me was the misleading documentation. I don't know exactly the cost of maintaining two schemas, but it's probably enough that it justifies not doing it.

---

_Closed by @charliermarsh on 2024-11-20 12:35_

---

_Comment by @MoElaSec on 2025-03-24 06:43_

so how will one be able to include `dev-dependencies` if it's not possible in `uv.toml` (I agree with the reasoning), and the presence of it leads to ignoring anything under the `[tool.uv]` in `pyproject.toml`, as we get with the warning:
```bash
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
```

or should this warning simply be ignored ?

---
