```yaml
number: 6511
title: Add first-class support for virtual projects
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-08-23T13:00:32Z
updated_at: 2024-08-29T13:57:13Z
url: https://github.com/astral-sh/uv/issues/6511
synced_at: 2026-01-12T15:59:05Z
```

# Add first-class support for virtual projects

---

_@charliermarsh_

A lot of users don't want to structure their projects as valid Python packages -- perhaps they're creating a web application, or a collection of scripts, etc. These all _can_ be structured as valid Python packages, but users tend not to do this. Requiring that projects are valid Python packages makes it harder for existing users to adopt uv.

Today, we allow "virtual projects" (i.e., without a build system) for workspace roots, but only for workspace roots.

This has several issues...

1. It _has_ to be a workspace, you need `[tool.uv.workspace]` which is confusing for users.
2. It only supports dev dependencies, also confusing for users.
3. There's no way to define project metadata which is confusing for users since they expect a `[project]` table. For example, there's no way to define a required python version in a "virtual project" (i.e., a virtual workspace root with no members).
4. There's (easy) no way to transition between the two. To convert a non-virtual `pyproject.toml` to virtual, you have to remove basically everything, move things around, etc.

What I want to do is, allow users to set:

```toml
[tool.uv]
virtual = true
```

If set, they can have a `[project]` table, they don't need a workspace definition, etc. And in this case, we don't build or install the project, just like a virtual workspace root today.

In the future, we'd like to write our own build system that handles these cases better by default. But for now, this will help a lot of users.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 13:00_

---

_Label `enhancement` added by @charliermarsh on 2024-08-23 13:00_

---

_Comment by @charliermarsh on 2024-08-23 13:00_

There's an argument to be made that this should be the _default_ if you don't define a build system. I'm also open to that.

---

_Comment by @charliermarsh on 2024-08-23 13:00_

I am going to prototype this later today.

---

_Comment by @chrisrodrigue on 2024-08-23 14:49_

Thanks Charlie. This will definitely help users working in monorepos and legacy codebases that contain many nested packages/scripts without clear boundaries for various components of their applications/libraries.

It’s an architectural issue for sure, and outside of massively refactoring products/projects, not really solvable in the short term, so it is nice to see `uv` bridging the gap.

---

_Comment by @charliermarsh on 2024-08-23 14:50_

Yeah I think this is something we missed on slightly in the initial release. I'm gonna prioritize it.

---

_Comment by @chrisrodrigue on 2024-08-23 14:55_

Rather than the venv-in-project approach (which I personally prefer), lot of users just set up various OCI images for various “environments” that have the python dependencies pre-installed in a custom virtual environment location.

These are effectively virtual projects, and it goes hand in hand with being able to define custom virtual environment locations (several issues about it #1495 / #5229 / #6504).

[Conda](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) is strong enabler of this type of workflow so having compatibility with it would ease migration for thousands of users. 

---

_Comment by @zanieb on 2024-08-23 14:56_

Please see #1495 for that — let's try to avoid expanding the scope of this discussion :)

---

_Comment by @charliermarsh on 2024-08-23 21:39_

Highly unlikely that this ships today, but I'm starting to work on it.

---

_Comment by @charliermarsh on 2024-08-24 16:13_

Coming together in https://github.com/astral-sh/uv/pull/6585.

---

_Comment by @bluss on 2024-08-24 18:14_

Using uv to "just manage my environment of dependencies" sounds natural and useful.

There was one kind of friction seen with Rye users with virtual projects, where they enable virtual project to achieve some goal (like not installing the root package, or not having to define how to create a proper wheel) but other things they want (pytest? editor support?) would benefit from installing the project as a python package in an environment.

I'm just wondering how to make it very clear what's happening so that the trade-offs are apparent to the user?

Are pipx and uv tool good points for why uv virtual project is not <=> "application", because you need applications distributed through pipx/uv tool to be "pip-installable"?

---

_Comment by @overfl0 on 2024-08-26 14:51_

> Coming together in #6585.

Hi, @charliermarsh does the above PR fix the "issue 2" mentioned in the first message so that you can have dev and non-dev dependencies?
Also, how complete is that PR? Is it functional enough to test uv built from that branch or are there key parts that are still missing? I saw the TODO checkboxes, but it's not really obvious to me how they map to actual functionality for the end-user.

I started porting my application from poetry using `package-mode = false` to uv and stumbled upon this very problem so it would be a PR that I'm eagerly awaiting.

---

_Closed by @charliermarsh on 2024-08-27 17:42_

---

_Closed by @charliermarsh on 2024-08-27 17:42_

---

_Comment by @philgyford on 2024-08-29 13:29_

v0.4.0 is a massive jump for those of us working on "application" projects, thank you!

But I still can't see how to install dev dependencies in my local virtualenv.

From the docs I see I can do `uv add ruff --dev` which will add it to `pyproject.toml`:

```toml
[tool.uv]
dev-dependencies = [
  "ruff",
]
```

But I don't know how to install them along with the non-dev `dependencies` in `[project]`. My first guess would be a `--dev` flag:

```shell
$ uv pip install -r pyproject.toml --dev
```

but that's not a valid argument.

---

_Comment by @weihenglim on 2024-08-29 13:38_

You should use `uv sync` or `uv run` instead of `uv pip install` to install dependencies from a `pyproject.toml`. You can include the `--no-dev` flag to exclude dev dependencies. 

---

_Comment by @zanieb on 2024-08-29 13:39_

@philgyford the `uv pip install` interface will not read non-standard project metadata (like development dependencies). And... @wimglenn said everything I was going to say :)

---

_Comment by @philgyford on 2024-08-29 13:57_

Ah, thank you! I obviously got confused, and am still not really clear about the difference. I'll have to read through all the docs again.

I was probably thrown by how I use pip-tools locally (although maybe I'm doing that wrong too!).

---
