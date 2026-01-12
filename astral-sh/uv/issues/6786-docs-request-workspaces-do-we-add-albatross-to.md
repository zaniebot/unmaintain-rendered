```yaml
number: 6786
title: "Docs request: Workspaces -- do we add `albatross` to `tool.uv.sources`"
type: issue
state: open
author: jamesbraza
labels:
  - documentation
assignees: []
created_at: 2024-08-29T01:37:19Z
updated_at: 2024-10-18T14:22:38Z
url: https://github.com/astral-sh/uv/issues/6786
synced_at: 2026-01-12T15:59:07Z
```

# Docs request: Workspaces -- do we add `albatross` to `tool.uv.sources`

---

_@jamesbraza_

With `uv==0.4.0` looking at https://docs.astral.sh/uv/concepts/workspaces/#workspace-layouts, it details:
- `albatross` in `src`
- `bird-feeder` in `packages/bird-feeder/src`
- `seeds` in `packages/seeds/src`

The docs don't mention if:

- `albatross` should be added to `tool.uv.sources`
- If `workspace = true` is required, and when to use `path`

The below is what I got working, but it does not match the example shown here: https://docs.astral.sh/uv/concepts/workspaces/#workspace-sources

```toml
[tool.uv.sources]
bird-feeder = {workspace = true}
albatross = {workspace = true}

[tool.uv.workspace]
members = ["packages/*"]
```

More specifically, `albatross = {workspace = true}` is not shown in the docs. But when I remove it and run `uv lock`, I get:

```none
error: Failed to build: `aviary-hotpotqa @ file:///path/to/repo/packages/bird-feeder`
  Caused by: Failed to parse entry for: `albatross`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

---

_Comment by @charliermarsh on 2024-08-29 01:40_

What do you have in the `pyproject.toml` for `file:///path/to/repo/packages/bird-feeder`?

---

_Label `question` added by @charliermarsh on 2024-08-29 01:40_

---

_Comment by @jamesbraza on 2024-08-29 01:42_

Here is that `pyproject.toml`:

```toml
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64", "setuptools_scm>=8"]

[project]
dependencies = [
    "albatross",
]
dynamic = ["version"]
name = "bird-feeder"
requires-python = ">=3.11"

[tool.ruff]
extend = "../../pyproject.toml"

[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools_scm]
root = "../.."
version_file = "src/bird-feeder/version.py"

```

---

_Comment by @charliermarsh on 2024-08-29 02:05_

Cool, so in _that_ `pyproject.toml`, you should include:

```toml
[tool.uv.sources]
albatross = { workspace = true }
```

The sources should be defined wherever the dependencies are included. Does that make sense?

---

_Comment by @jamesbraza on 2024-08-29 02:07_

Yeah that makes sense! Thanks for working through it

I think then we should update the example `pyproject.toml`'s in https://docs.astral.sh/uv/concepts/workspaces/#getting-started to include that

---

_Comment by @charliermarsh on 2024-08-29 02:11_

Good idea!

---

_Label `question` removed by @charliermarsh on 2024-08-29 02:12_

---

_Label `documentation` added by @charliermarsh on 2024-08-29 02:12_

---

_Comment by @thorbenk on 2024-10-18 12:57_

> Cool, so in _that_ `pyproject.toml`, you should include:
> 
> ```toml
> [tool.uv.sources]
> albatross = { workspace = true }
> ```
> 
> The sources should be defined wherever the dependencies are included. Does that make sense?

OK, that matches up with the source code in https://github.com/astral-sh/uv/blob/main/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder/pyproject.toml.

Then I have a question: Is it not OK to move the `tool.uv.sources` declaration to the root like in this commit: https://github.com/thorbenk/uv-workspaces-question/commit/0ca5611a69c6f5c77ad1d09cb2589291c11974e9

I want to achieve the following:
- each package does not itself know that it is part of a workspace (actually, each package for me is a git submodule, and released as an independent python package)
- Only the workspace root ties together all packages into a workspace

Am I misunderstanding the workspace concept?

---

_Comment by @charliermarsh on 2024-10-18 13:41_

@thorbenk -- I think that should work because members inherit sources from the root. Did it not work?

---

_Comment by @thorbenk on 2024-10-18 14:22_

@charliermarsh: Sorry, I did not write that clearly.

Yes, it works with my changes, too.
However, I was wondering if this was intentional.
Happy to hear that it is!

Actually, I was using the setup like above and it was working well for me in an internal project.
Then today I ported to using the new explicit indexes and I ran into issues, prompting me to wonder whether I was using supported behavior.

I will try to create a minimal reproducible example for this.

Thanks for an amazing `uv` experience!



---
