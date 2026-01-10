```yaml
number: 9150
title: Workspace per project lockfile and virtual environment
type: issue
state: open
author: dudicoco
labels:
  - needs-decision
assignees: []
created_at: 2024-11-15T14:36:35Z
updated_at: 2025-12-02T22:50:35Z
url: https://github.com/astral-sh/uv/issues/9150
synced_at: 2026-01-10T03:23:53Z
```

# Workspace per project lockfile and virtual environment

---

_Issue opened by @dudicoco on 2024-11-15 14:36_

Hi,

Currently, in workspace mode uv will install a virtual environment and will create a global lock file in the project root.

Please allow to have a separate .venv and uv.lock per project.

The use case for this is that in a monorepo you might want to have strict separate dependencies per project but have the ability to efficiently resolve and install the dependencies in one command.

This is how pnpm workspace install works with the 	[shared-workspace-lockfile](https://pnpm.io/next/npmrc#shared-workspace-lockfile) option set to false.




---

_Label `needs-decision` added by @charliermarsh on 2024-11-18 01:15_

---

_Comment by @md384 on 2025-02-05 14:35_

+1 on this, we have a monorepo with many services (and libraries and tools) where each need an individual lock file. Currently we have custom scripts around poetry and are considering switching to uv but without multiple lockfile support we'd still be stuck with wrapper scripts. uv supporting this natively would make switching a no-brainer.

---

_Comment by @truonghm on 2025-07-04 09:37_

Not sure if anyone else have this usecase but per-member lockfiles would also make deploying with Docker easier for me.
Currently I have to use a pre-commit hook to generate and check per-member `requirements.txt` files and use them to build each service.

Alternatively I can set the build context for each container to the root directory which contains `uv.lock`, but for my case, that would lead to some other complications. Ultimately I decided to set the build context to each member directory and as such also need a lockfile for each directory.

So yes I support an option to have per-workspace-member lockfile, while also keeping a global lockfile that synchronizes with all the members.

---

_Comment by @US579 on 2025-08-29 11:19_

+1 are there any update on this?

---

_Comment by @jmejco on 2025-09-15 16:37_

I have also run into this requirement of needing a workspace member lockfile. My project is a CDK monorepo with a shared library, a fastapi, and a click project as members. The API and CLI depend on the shared library. The API is deployed as an ECS task, and the CLI is used for Batch jobs. The shared library is used as a lambda layer, so I either need to generate the uv.lock as part of the CDK asset bundling, or need to use wrapper scripts to be able to generate and commit the member lock.

Any plans to support this natively?

---

_Comment by @TG-Techie on 2025-11-20 05:33_

I am also encountering an issue with this. We want to run each workspace member in their own docker instance with locked dependencies (of course ) but cannot generate the per-member lock files. What we're doing now is temporarily renaming the workspace pyproject.toml to something else then un-re-naming it

---

_Comment by @TG-Techie on 2025-11-20 06:09_

I whipped this(gist below) up to help, but I think there should be support for a non-hacky solution 

https://gist.github.com/TG-Techie/545ed4ef88186d5e928fcc33ff789fbc

---

_Comment by @rafaelgildin on 2025-12-01 14:48_

Hi guys, any updates on this pls? 

---

_Comment by @kadosh1000 on 2025-12-02 22:45_

Same issue here, we have a few services and a directory of packages, not all services are using all packages, but due to the lockfile, a change in any package fully invalidates the docker cache.

Feels like a very reasonable scenario, and should be supported by simply running `uv lock` from the service directory. 
It will probably also expedite the resolution process of the workspace root, because it won't need to re-resolve service dependencies.

@TG-Techie did you manage to make it work with a package using another workspace package? 
I am getting
```
  × Failed to build `api @ file:///Users/xxxxxx/Desktop/repo/services/api`
  ├─▶ Failed to parse entry: `auth`
  ╰─▶ `auth` references a workspace in `tool.uv.sources` (e.g., `auth = { workspace = true }`), but is not a workspace member
```

---
