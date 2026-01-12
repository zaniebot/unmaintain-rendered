```yaml
number: 6832
title: "`uv lock` doesn't work with packages containing period in name"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2024-08-29T21:13:51Z
updated_at: 2024-09-03T01:21:40Z
url: https://github.com/astral-sh/uv/issues/6832
synced_at: 2026-01-12T15:59:08Z
```

# `uv lock` doesn't work with packages containing period in name

---

_@jamesbraza_

I have a workspace that looks like this:

```none
.
├── packages
│  └── seeds
│     ├── src
│     │  └── albatross
│     │     └── seeds
│     │        ├── __init__.py
│     │        ├── stub.py
│     │        ├── py.typed
│     │        └── version.py
│     └── pyproject.toml
├── src
│  └── albatross
│     ├── stub.py
│     ├── py.typed
│     └── version.py
├── pyproject.toml
└── uv.lock
```

And the name of the `seeds` package is `albatross.seeds`.

In my repo root's `pyproject.toml`, I have:

```toml
[tool.uv.sources]
albatross = {workspace = true}
"albatross.seeds" = {workspace = true}
```

Running `uv lock`, it:
- Contains `albatross-seeds` not `albatross.seeds`
- Running `pip list` I don't see `albatross-seeds` or `albatross.seeds` anywhere

Here's a subset of the `uv.lock`:

```none
version = 1

[manifest]
members = [
    "albatross",
    "albatross-seeds",
]

[[package]]
name = "albatross"
version = "0.1.dev7+gbe81ca2.d20240829"
source = { editable = "." }
dependencies = []

[package.dev-dependencies]
dev = [
    { name = "pytest" },
]

[package.metadata]
requires-dist = []

[package.metadata.requires-dev]
dev = [
    { name = "pytest", specifier = ">=8" },
]

[[package]]
name = "albatross-seeds"
version = "0.1.dev7+ga33449c"
source = { editable = "packages/seeds" }
dependencies = [
    { name = "albatross" },
]

[package.metadata]
requires-dist = [
    { name = "albatross", editable = "." },
]
```

---

_Comment by @charliermarsh on 2024-08-29 22:37_

It’s normal for us to normalize the name like that. (Those are equivalent names from the perspective of the standards — they map to the exact same package.) Though it’s confusing that you aren’t seeing it in the environment at all? I’ll try to repro.

---

_Comment by @jamesbraza on 2024-08-29 23:04_

> It’s normal for us to normalize the name like that.

Sounds good, though ideally the names exactly match for debugging ease.

> Though it’s confusing that you aren’t seeing it in the environment at all? I’ll try to repro.

Yeah when I run `uv sync` the name `albatross-seeds` does not show up. I have to manually `uv pip install` it for it to work

---

_Comment by @charliermarsh on 2024-08-29 23:31_

Oh sorry, just to clarify, is `albatross.seeds` declared as a dependency of `albatross`? You might be missing this from the `albatross` `pyproject.toml`:

```toml
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["albatross.seeds"]
```

---

_Comment by @jamesbraza on 2024-08-30 00:40_

> is `albatross.seeds` declared as a dependency of `albatross`?

Yeah it is included as a dependency 👍 thanks for checking. That's not it in this case

---

_Comment by @charliermarsh on 2024-08-30 02:22_

Ok thanks! I’ll take a look tomorrow.

---

_Comment by @charliermarsh on 2024-08-30 12:57_

Does `packages/seeds/pyproject.toml` contain a build system? Do you mind sharing that section?

---

_Comment by @jamesbraza on 2024-08-30 17:20_

> Does `packages/seeds/pyproject.toml` contain a build system? Do you mind sharing that section?

Yes it does, here's a minimal `packages/seeds/pyproject.toml`:

```toml
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64", "setuptools_scm>=8"]

[project]
dependencies = ["albatross"]
dynamic = ["version"]
name = "albatross.seeds"

[tool.ruff]
extend = "../../pyproject.toml"

[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools_scm]
root = "../.."
version_file = "src/seeds/albatross/version.py"
```

---

_Comment by @charliermarsh on 2024-08-30 17:27_

Thanks. Sorry for all the back-and-forth, I'm just having trouble reproducing:

```
❯ uv sync
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved 2 packages in 2ms
Installed 2 packages in 1ms
 + albatross==0.1.0 (from file:///Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-root-workspace)
 + albatross-seeds==0.1.dev0+d20240830 (from file:///Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-root-workspace/packages/seeds)

❯ uv pip list
Package         Version            Editable project location
--------------- ------------------ ------------------------------------------------------------------------------------------
albatross       0.1.0              /Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-root-workspace
albatross-seeds 0.1.dev0+d20240830 /Users/crmarsh/workspace/puffin/scripts/workspaces/albatross-root-workspace/packages/seeds
```

Are you using `pip list` or `uv pip list`?

---

_Comment by @jamesbraza on 2024-08-30 18:07_

Both `pip list` and `uv pip list` don't show `albatross.seeds` or `albatross-seeds`.

Here is my `albatross`'s `pyproject.toml`:

```toml
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64", "setuptools_scm>=8"]

[project]
dependencies = []
dynamic = ["version"]
name = "albatross"

[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools_scm]
version_file = "src/albatross/version.py"

[tool.uv]
dev-dependencies = [
    "build",  # TODO: remove after https://github.com/astral-sh/uv/issues/6278
    "pytest>=8",  # Pin to keep recent
]

[tool.uv.sources]
"albatross.seeds" = {workspace = true}
albatross = {workspace = true}

[tool.uv.workspace]
members = ["packages/*"]
```

Does that help?

What I can do to force the install is:

```bash
uv sync
uv pip install -e packages/seeds
```

---

_Comment by @charliermarsh on 2024-08-30 19:43_

Try this:

```toml
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64", "setuptools_scm>=8"]

[project]
dependencies = ["albatross.seeds"]
dynamic = ["version"]
name = "albatross"

[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools_scm]
version_file = "src/albatross/version.py"

[tool.uv]
dev-dependencies = [
    "build",  # TODO: remove after https://github.com/astral-sh/uv/issues/6278
    "pytest>=8",  # Pin to keep recent
]

[tool.uv.sources]
"albatross" = { workspace = true }
"albatross.seeds" = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
```

---

_Comment by @charliermarsh on 2024-08-30 19:44_

I believe you're missing `dependencies = ["albatross.seeds"]` in `albatross/pyproject.toml`

---

_Comment by @jamesbraza on 2024-08-30 20:38_

Oh but `albatross` doesn't depend on `albatross.seeds`, just `albatross.seeds` depends on `albatross`. So that was not it

If you can't repro, I can add you to the private repo to take a look. Are you open to that?

---

_Comment by @charliermarsh on 2024-08-30 20:45_

Sure, I'm happy to take a look!

Can you clarify which command you're running, and from which directory, that you expect to get `albatross.seeds` into the environment? Running `uv sync` from the root will not install `albatross.seeds` if the root doesn't depend on it. You'd need to run `uv sync --package albatross.seeds` or `uv sync` from the `seeds` directory.

---

_Comment by @jamesbraza on 2024-08-30 21:01_

> Running `uv sync` from the root will not install `albatross.seeds` if the root doesn't depend on it. You'd need to run `uv sync --package albatross.seeds` or `uv sync` from the `seeds` directory.

Ahhhhh okay that was my misunderstanding, I was thinking for workspaces that `uv sync` would automatically pull in the entire workspace.

Do you think:
- `uv sync` should pull in the full workspace by default? This seems intuitive to me
- Can we add a boolean flag to `tool.uv.sources` that marks workspaces to be included in a bare `uv sync`

Or we should document in the workspaces docs that `uv sync` doesn't pull in all workspaces by default.

---

Two workaround are:

- Using `--package` arg to `uv sync` (as you suggest): `uv sync --package albatross.seeds`
- Adding `albatross.seeds` to main package's `dev-dependencies`

---

_Comment by @charliermarsh on 2024-09-03 01:21_

On that first question, see: https://github.com/astral-sh/uv/issues/5727. I just reopened it since it's now come up a few times.

---

_Closed by @charliermarsh on 2024-09-03 01:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-03 01:21_

---

_Label `question` added by @charliermarsh on 2024-09-03 01:21_

---
