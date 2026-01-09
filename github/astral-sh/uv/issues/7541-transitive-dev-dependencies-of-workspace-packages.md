---
number: 7541
title: Transitive dev dependencies of workspace packages are not installed
type: issue
state: closed
author: lukasbindreiter
labels:
  - duplicate
assignees: []
created_at: 2024-09-19T10:56:10Z
updated_at: 2024-11-20T00:38:03Z
url: https://github.com/astral-sh/uv/issues/7541
synced_at: 2026-01-07T13:12:17-06:00
---

# Transitive dev dependencies of workspace packages are not installed

---

_Issue opened by @lukasbindreiter on 2024-09-19 10:56_

I just ran into an issue in our CI pipeline (after upgrading to `uv 0.4.10`), which I believe was caused by this change:
https://github.com/astral-sh/uv/pull/7318

I agree that theoretically it makes sense to not install dev dependencies transitively, however that may be different for workspace sources in a monorepo.

This is the outline for the issue that came up:
We have a monorepo, using `uv workspaces`.
In there, we have multiple packages, all part of the same `workspace`. The root package has a dependency on all of those, so that their dependencies are all installed into the `venv` transitively.
Now all of the packages in the monorepo have different `dev` dependencies, one of those for example [moto](https://pypi.org/project/moto/).

Before this change, I was able to run tests by doing `uv run pytest <package>`, because the root `venv` also had all dev-dependencies installed. Now this is no longer the case, instead I get a `ModuleNotFoundError: No module named 'moto'`.

Should we now specify all `dev` dependencies of the sub-packages also as `dev` dependency of the root package?
Or maybe it would make sense to still install `dev` dependencies transitively for all packages where `workspace = true` is set in `tool.uv.sources`?

---

_Comment by @zanieb on 2024-09-19 11:07_

Sorry this broke your setup!

> Should we now specify all dev dependencies of the sub-packages also as dev dependency of the root package?

I think this is the recommendation, if you want to use them from the workspace root.

Otherwise, you can use `uv run --package <name> pytest ...` to run with the development dependencies of a workspace member.

@charliermarsh may have more thoughts.

---

_Comment by @johnhoran on 2024-09-19 18:40_

Could we have a flag colocated with `workspace = true` to decide if the dev dependences should be added?

---

_Comment by @lukasbindreiter on 2024-09-20 08:54_

> Otherwise, you can `use uv run --package <name> pytest ...`

Thank you - this worked for us, and it's now what I did to resolve the issue for now.

However, now after running tests, and then doing an `uv sync` will uninstall all the dev dependencies again. Which makes sense, but also feels a bit weird.

The reason we prefer to have the `dev-dependencies` separate for each package is that they should be as standalone as possible, e.g. if at any point we want to make them their own repositories instead they should be fully self-contained.

> Could we have a flag colocated with `workspace = true` to decide if the dev dependences should be added?

I like that suggestion a lot, e.g. something like:

```
[tool.uv.sources]
package-a = { workspace = true, include_dev = true }
package-b = { workspace = true, include_dev = true }
ruff = { include_dev = true }  # if I want for whatever reason we could maybe also include dev dependencies of pypi packages?
```

what do you think?

---

_Comment by @johnhoran on 2024-09-20 11:31_

For right now my approach is to put all the dev packages into an optional package and then include that in the dev dependencies of itself and the root package.

---

_Comment by @BurntSushi on 2024-09-20 11:56_

#7487 seems related to this issue, although perhaps not exactly duplicative. It sounds like this is about adding dev-dependencies from packages in a workspace to the root, where as #7487 is about the reverse: adding dev-dependencies from the root to each package.

---

_Comment by @blakeNaccarato on 2024-09-21 17:25_

I think this also gets at competing notions of (1) workspace members as strict dependencies of the root, (2)  workspace members as composable "mix-ins", and (3) a pseudo-rootless monorepo with first-class, root-like workspace members.

In (1), workspace members should be proper dependencies (dev or otherwise) of the root, and the root shouldn't have to "know" anything about workspace member deps, aside from the fact that all members must "live" in a shared environment. Making this assumption also probably simplifies `uv`'s solver?

In (2), a workspace member may function as a mix-in, in which case its dev dependencies augment the root's dev dependencies. You could imagine the isolation of different (yet not incompatible) packages isolated as workspace members which are pluggable and augment the root.

Then in (3) OP's monorepo layout, an ephemeral "root" but the workspace members that want to share their tooling, almost as if it were a "rootless" shared environment with all workspace members being kind of like their own first-class root.

I think we can agree that in any case, the proper `project.dependencies` of root and workspace members should be properly isolated and unaffected by each other, aside from having to solve to coexist in a single environment.

And in (2) and (3) you have the question of "what about `constraint-dependencies`, etc. etc.

I wonder if the `workspace` concept may be too overloaded in handling all of the dependent/mixin/monorepo use cases, or if enough config flags can be tracked on to `source` tables to satisfy everyone. And even if workspaces do grow enough toggles in `tool.uv.sources` entries to support cases (1)/(2)/(3)/(etc.), that comes at the cost of more boilerplate.

I like the globbing in `tool.uv.workspace`, possibly some config could "group" different `tool.uv.sources` with some common config, toggling e.g. `workspace = true` and `shared-dev-dependencies = true` or w/e on all members of the group.

---

_Referenced in [astral-sh/uv#8353](../../astral-sh/uv/issues/8353.md) on 2024-10-19 05:49_

---

_Comment by @bugzpodder on 2024-10-21 22:37_

it would be nice to have a flag that installs everything (all devDependencies in all projects and root workspace).  this is how things work by default in javascript land (npm/yarn/pnpm/bun/etc)

---

_Label `duplicate` added by @zanieb on 2024-10-21 22:42_

---

_Comment by @zanieb on 2024-10-21 22:43_

See also

- https://github.com/astral-sh/uv/issues/8002
- https://github.com/astral-sh/uv/issues/6935

---

_Comment by @charliermarsh on 2024-11-20 00:38_

We now have `uv sync --all-packages`, which I think satisfies this use-case.

---

_Closed by @charliermarsh on 2024-11-20 00:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 00:38_

---

_Referenced in [astral-sh/uv#7487](../../astral-sh/uv/issues/7487.md) on 2025-08-12 18:29_

---

_Referenced in [astral-sh/uv#15656](../../astral-sh/uv/issues/15656.md) on 2025-09-03 12:05_

---

_Referenced in [vortex-data/vortex#4696](../../vortex-data/vortex/pulls/4696.md) on 2025-09-18 16:03_

---
