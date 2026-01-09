---
number: 7314
title: Support working on multiple projects in the same venv
type: issue
state: open
author: cdwilson
labels:
  - question
assignees: []
created_at: 2024-09-11T22:47:10Z
updated_at: 2025-10-21T21:25:32Z
url: https://github.com/astral-sh/uv/issues/7314
synced_at: 2026-01-07T13:12:17-06:00
---

# Support working on multiple projects in the same venv

---

_Issue opened by @cdwilson on 2024-09-11 22:47_

I'd like to be able to install multiple projects in "development mode" (editable install) into the same virtual environment.

For example, let's say I'm developing projects `A`, `B`, and `C` simultaneously. Each of these projects are stored in separate git repos, published separately to PyPI, etc. However, the following dependencies exist:

- `B` depends on `A`
- `C` depends on `B`

When working on `A`, I'd like to use a venv where `A` is installed in "development mode" (editable install).

When working on `B`, I'd like to use a venv where `A` and `B` are installed in "development mode" (editable install).

When working on `C`, I'd like to use a venv where `A`, `B`, and `C` are installed in "development mode" (editable install).

I tried adding the dependencies for each project to `[tool.uv.sources]` by specifying a relative path for each. While this appears to be the solution, I don't really want to commit this into git because it requires anybody else who clones the project to have the dependencies located in the exact same relative paths.

I think what I might be looking for is some way to specify a set of `[tool.uv.sources]` for each project that are only local to my machine (not checked into git).

**Is there a recommended way to achieve this workflow using uv?** (It's also totally possible my workflow is bad and I'm missing the obvious correct way to work on multiple packages simultaneously.)

BTW, I'm coming from pyenv where this was simple to do. I could just create a single virtualenv for each project (which was stored outside of the project dir) and do an editable install of all dependencies into the active virtualenv.


---

_Comment by @zanieb on 2024-09-12 00:57_

> I could just create a single virtualenv for each project (which was stored outside of the project dir) and do an editable install of all dependencies into the active virtualenv.

So, we still support this via `uv venv` and `uv pip install -e` but if you want to use a higher-level API then I think you want to declare a workspace.

I think the idea would roughly be to a create an empty project, clone the three projects as subdirectories (or link them?), and add them as dependencies / workspace members to the root workspace project. Does that make sense?

The workspace documentation is available at https://docs.astral.sh/uv/concepts/workspaces/


---

_Label `question` added by @zanieb on 2024-09-12 00:57_

---

_Label `question` added by @zanieb on 2024-09-12 00:57_

---

_Label `question` added by @zanieb on 2024-09-12 00:57_

---

_Comment by @cdwilson on 2024-09-12 01:26_

> So, we still support this via uv venv and uv pip install -e

The issue I ran into with this is that when `A` is installed via `pip install -e` into `B`'s virtual environment, running `uv sync` in `B` will remove the editable install of `A` (because it is not listed as a dependency in `B`'s `pyproject.toml`).

> I think you want to declare a workspace.

The issue with workspaces is that it seems to require that each project is a subdirectory of some top-level workspace project.

Imagine the example above, but extended with another project `D`, where `D` also depends on `A`, but is totally unrelated to `B` or `C`. In this case putting everything into the same workspace doesn't make much sense.

To work on `D`, I would have to create a second workspace project with a second copy of `A` in a subdirectory of `D`'s workspace. Now, I have to maintain separate copies of all the same dependencies in each workspace. As you suggested, maybe symlinking the dependencies from a central location instead of copying them might work, but it starts to feel like a lot of overhead vs. what I can do very easily/simply with pyenv.

Ideally, I'd like to just have a single development copy of `A` on my system, but install it as an editable-install in multiple other project's virtualenvs. I suppose this would require some user-specific (i.e. not checked into git) mechanism outside of the `pyproject.toml` file to record these dependency sources.

I'm totally open to changing my workflow, but just trying to find a way that is similarly low overhead to what I'm doing now.


---

_Comment by @zanieb on 2024-09-12 01:33_

Yeah you can't really intermix `uv sync` and `uv pip install`. You can use `uv sync --inexact` to retain the existing install if you want to mix them, but they're generally not designed for that.

> I'm totally open to changing my workflow, but just trying to find a way that is similarly low overhead to what I'm doing now.

I feel that. We're happy to help! I'm not sure if we have great support for the situation you've described yet. @konstin might have some better ideas for you. We've also considered ways to make it easier to "patch" dependencies of projects with local versions, but I think in that case we'd still require modifying the `pyproject.toml`.



---

_Comment by @cdwilson on 2024-09-12 01:37_

> We're happy to help!

Thanks for the ideas and help so far! I'll try out the workspace/symlink idea you suggested and see how that works out.

---

_Comment by @cdwilson on 2024-09-12 02:19_

> We've also considered ways to make it easier to "patch" dependencies of projects with local versions, but I think in that case we'd still require modifying the pyproject.toml.

Now that you mention it, I think this workflow is similar (if not essentially the same) to what I'm looking for, i.e. the ability to "patch" a project with a local copy of a dependency (usually via an editable install). It seems like there are two use cases in both of these workflows:

1. Dependency sources need to be recorded in `pyproject.toml` and checked into git (implying that [Path dependency sources](https://docs.astral.sh/uv/concepts/dependencies/#path) always have the same hard-coded paths on every system)
2. Dependency sources need to be configured locally, but Path dependency sources may vary across systems (i.e. not checked into git).

I don't have a sense for how much of an edge case the 2nd one of these is or whether it's worth supporting in some way.

---

_Comment by @juan11iguel on 2024-09-17 08:30_

I am also facing this issue, I would like to have a "development" environment where some (editable) dependencies are local paths, and a "production" environment where the sources are different (a git repo or any other really). All handled via the same `pyproject.toml`. 

A possible interface might be:

```toml
[project]
...
dependencies = [
    "numpy>=1.26.4",
    "pandas>=2.2.2",
    ...,
    "project_A",
    "project_B"
]

[tool.uv]
dev-dependencies = [
    "project_A",
    "project_B",
]

[tool.uv.sources]
project_A = { git = "https://github.com/username/project_A", branch = "main", "dev": { path = "../project_A", editable = true} }
project_B = { git = "https://github.com/username/project_B", branch = "main", "dev": { path = "../project_B", editable = true} }
```

Here for the same dependency there is a default source and and a `dev` source that might be used when the project is run with a `--dev` argument

---

_Comment by @ckutlu on 2025-04-30 12:28_

We have a similar workflow: we have A and B where B depends on A and we would like to quickly iterate on changes of A when developing B. We are doing what @zanieb suggested:

> So, we still support this via `uv venv` and `uv pip install -e` ...

We are looking for a lightweight solution, so telling people "put A and B into some folder and keep a pyproject.toml there that declares A and B as workspace packages" would be fine. I.e.:

> but if you want to use a higher-level API then I think you want to declare a workspace.

This approach didn't work for us unfortunately because A has its own workspace and currently nested workspaces aren't supported.

Not sure how well it would work, but I'm considering adding A as a path based development dependency _as well_ to B, i.e. `uv add --editable ../A`. Haven't played with this yet, but I'm thinking there may be issues because A build is relatively complicated with multiple rust subpackages 🤔 

---

_Comment by @lascott on 2025-06-25 18:16_

Consider the case where each project has its own .env, and projects A,B and C are all quite similar in their dependencies.
Q1: Am I downloading the dependencies once or three times? 
Q2: A predates B by three months, C predates B by three months. But I have modified each in the last week.
       Now how many dependencies am I generating not using a workspace?

---

_Referenced in [thromer/uv-monorepo-example#1](../../thromer/uv-monorepo-example/pulls/1.md) on 2025-06-29 21:52_

---

_Referenced in [datalab-org/datalab-app-plugin-insitu#48](../../datalab-org/datalab-app-plugin-insitu/issues/48.md) on 2025-07-14 10:29_

---

_Comment by @joeyjurjens on 2025-10-21 21:25_

I have a similar use case coming from Buildout. We maintain a “kitchensink” development environment where we need to work on multiple packages simultaneously, each in their own separate Git repository.

In Buildout, we used the `[sources]` section with `mr.developer` to:

- Clone all Git repos into a `sources/` directory
- Automatically install them all as editable/development mode
- Allow developers to commit changes to any package directly

The current `[tool.uv.sources]` approach doesn’t work for this because:

- Git sources install from the remote repo (not editable)
- Path sources require repos to already be cloned and hardcode relative paths (which we don’t want committed to git)

Maybe `[tool.uv.sources]` can be extended to support cloning Git repos locally and installing them as editable. For example:

```toml
[tool.uv.sources]
package-a = { git = "https://github.com/org/package-a", dest = "sources/", editable = true }
package-b = { git = "https://github.com/org/package-b", dest = "sources/", editable = true }
```

This would clone the repos to the specified path if they don’t exist, then install them as editable from the local path.

Above is just an idea, no idea how doable and good of an approach it would actually be.

---
