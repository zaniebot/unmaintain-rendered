```yaml
number: 7945
title: "Modifying `tool.uv.sources` between production and development"
type: issue
state: open
author: iBuitron
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-10-06T05:59:11Z
updated_at: 2025-11-10T16:09:35Z
url: https://github.com/astral-sh/uv/issues/7945
synced_at: 2026-01-12T15:59:18Z
```

# Modifying `tool.uv.sources` between production and development

---

_@iBuitron_


Hi!

this is my pyproject.toml

```
[project]
name = "quickbio"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pydantic>=2.9.2",
    "pydantic-settings>=2.5.2",
    "data-management-tools",
    "quickbio-books",
    "quickbio-diversity",
    "pandera>=0.20.4",
    "pydantic-extra-types>=2.9.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
data-management-tools = { path = "../../data_management_tools", editable = true }
quickbio-books = { path = "../quickBio-books", editable = true }
quickbio-diversity = { path = "../quickbio_diversity", editable = true }
```
There is any way to use 
```
data-management-tools
quickbio-books
quickbio-diversity 
```

with git urls at the same time as local path.

What I mean is that I want to specify both paths, but for example when I'm working locally I use ../path, but when I use docker I use the git paths.
Is there a way to do that?

---

_Comment by @zanieb on 2024-10-06 14:44_

Are you developing on a different platform than your Docker container? Like developing on macOS but using Linux for Docker?

---

_Comment by @iBuitron on 2024-10-06 16:16_

No, all in Ubuntu.

I have a script that changes dependencies when I'm on one branch or another, for example:
If I'm on the dev branch, it uses the local path (in the pyproject.toml)
If I run the script (specifying the "main" branch) it overwrites the pyproject.tom changing the local paths to git paths.

What I want to know is if there is a way to avoid using the script I have, and for example, reading an environment variable, and thus using a group of dependencies (Git) over others (Local)

dev branch:
```
data-management-tools = { path = "../../data_management_tools", editable = true }
quickbio-books = { path = "../quickBio-books", editable = true }
quickbio-diversity = { path = "../quickbio_diversity", editable = true }
```

run script (and change to main):

```
data-management-tools = { git= "GITPATH" }
quickbio-books = { git= "GITPATH" }
quickbio-diversity = { git= "GITPATH" }
```

---

_Comment by @charliermarsh on 2024-10-08 22:39_

You might be able to do this with `--no-sources`? Like:

```toml
[project]
name = "quickbio"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "data-management-tools @ git+ GITPATH",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
data-management-tools = { path = "../../data_management_tools", editable = true }
```

Then `uv sync` would install as editable, but `uv sync --no-sources` should install from Git.

---

_Label `question` added by @charliermarsh on 2024-10-08 22:39_

---

_Closed by @charliermarsh on 2024-10-11 09:47_

---

_Comment by @iBuitron on 2024-10-14 03:18_

@charliermarsh hi!
Doesn't work :/

```
[project]
name = "quickbio-books"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "dash>=2.18.1",
    "httpx>=0.27.2",
    "odmantic>=1.0.2",
    "pydantic>=2.9.2",
    "pydantic-settings>=2.5.2",
    "xmltodict>=0.13.0",
    "pandas>=2.2.3",
    "tabula>=1.0.5",
    "pyreadr>=0.5.2",
    "pyarrow>=17.0.0",
    "dev-tools @ git+https://github.com/iBuitron/dev_tools.git",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true

[tool.uv.sources]
dev-tools = { path = "../../dev_tools", editable = true }

```

```
reveur@italobuitron:~/python/environmental/quickbio_books$ uv sync
error: Failed to build: `quickbio-books @ file:///home/reveur/python/environmental/quickbio_books`
  Caused by: Failed to parse entry for: `dev-tools`
  Caused by: Can't combine URLs from both `project.dependencies` and `tool.uv.sources`
```
`uv sync --no-sources Works`



---

_Comment by @charliermarsh on 2024-10-14 14:04_

\cc @konstin -- do you think we should allow this? I kind of do.

---

_Reopened by @charliermarsh on 2024-10-14 14:04_

---

_Renamed from "Usea local and git paths" to "Mixing local and Git paths between production and development" by @charliermarsh on 2024-10-14 14:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-14 16:09_

---

_Comment by @iBuitron on 2024-10-14 16:46_

For example.
Right now I'm using my own libraries that I use locally and that I change/evolve according to my needs so as not to have all my code coupled.

quickbio_book depends on dev_tools
quickbio depends on both quickbio_book and dev_tools
and so on, all locally, all in --editable mode.

but right now the limitation I have is that if I wanted to test it in docker, I would have to manually change all the local paths to the github repositories, and once tested, if I wanted to change something I would have to change it back to the local paths and then (again) change it to github to test it again in docker (so, for each of my libraries).

I do all of this with a script, which is responsible for making all the changes and even uploading them to github, but I was wondering if there is a way for UV to take care of it.

Maybe by creating a group where the libraries are specified in editable mode (dev) and in production mode, or by allowing you to read an environment variable with which UV allows you to use one over the other (like the marker platform).

```
[project]
dependencies = [
  "httpx",
]


[tool.uv.sources]
httpx = [
  { path = "../.../httpx",  custom_env_var= "dev" },
  { git = "https://github.com/encode/httpx", tag = "0.24.1", custom_env_var = "prod" },
]
```

If I set the environment variable "custom_env_var" == "dev" it would use the local.
If I set the environment variable "custom_env_var" == "prod" it would use the git path.


Of course, this is thought from my point of view in the development of my application (I am not a programmer as such, I am a Biologist who learned to program, maybe I am missing some more efficient way of doing it according to good practices) but it is what I have seen that has limited me and that perhaps UV can handle it.

Thank you for your time :)


---

_Comment by @konstin on 2024-10-14 17:20_

> do you think we should allow this? I kind of do.

From a technical perspective, yes we can support this.

> Maybe by creating a group where the libraries are specified in editable mode (dev) and in production mode, or by allowing you to read an environment variable with which UV allows you to use one over the other (like the marker platform).

I'm trying to understand how the path/git workflow works in practice, specifically keeping things in sync. E.g., how are you making sure that quickbio and data-management-tools are in sync when deploying? Let's say you add a function `foo` to data-management-tools in the repo, and in the same commit start using it in quickbio. When deploying to docker, you could have an older git version of data-management-tools without `foo`, while the path version of quickbio expects `foo`.

---

_Comment by @iBuitron on 2024-10-14 19:19_

First I will explain the way I do it currently, which is a bit complicated since there are several steps (although I have it automated and it works well for me)

What I do is.

```
v.1.1.1
- branch "dev"
quickbio ( localpath --editable)
dmt (localpath --editable)
```

- commit to github

I have to create another branch called "prod" where I merge the "dev" branch to change the dependency paths here

```
v.1.1.1
- branch "prod"
quickbio (gitpath)
dmt (gitpath)
```

- commit to github

So far, I have two branches, "dev" and "prod" both, with the same code, the only difference is the path of each dependency.

In docker, I use the "prod" branch so that the dependencies are installed from github using the gitpath

Now, we added a new feature

```
v.1.1.2
- branch "dev"
quickbio ( localpath --editable)
dmt with FOO (localpath --editable)
```

- commit to github (again)

```
v.1.1.2
- branch "prod"
quickbio (gitpath)
dmt with FOO (gitpath)
```

- commit to github (again)

So, in docker, the dependencies would be installed from github since the gitpath is still the same (and is updated with the latest commit)

Now, how do I see that this is solved?

I specify the paths for the same dependency

```
dmt = [
{ path = "../../dmt, editable = true, custom_env_var="dev"},
{ git = "https://github.com/iBuitron/dmt.git", custom_env_var="prod"}
]
```
Note: The branch should be specified to avoid conflicts


By default, the custom_env_var would be "dev", but in docker, if I wanted to use the git path, I would set custom_env_var = "prod"


---

_Comment by @zanieb on 2024-12-03 18:00_

I also tried to do this in an example for someone today

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx @ git+https://github.com/encode/httpx",
]

[tool.uv.sources]
httpx = { path = "../../httpx", editable = true }
```

There was a discussion in Discord in which someone wants to use a workspace source during development but a built wheel path in production. We don't have a dev / prod toggle, you'd have to hack `extra` to do this?

---

_Comment by @choucavalier on 2024-12-03 22:07_

I second the need for this functionality. Having some libraries as editable during development and correctly packaging them from git sources for CI/CD pipelines would be great. Right now there's no solution for this.

---

_Renamed from "Mixing local and Git paths between production and development" to "Modifying `tool.uv.sources` between production and development" by @charliermarsh on 2024-12-06 11:58_

---

_Comment by @charliermarsh on 2024-12-06 11:59_

Another example here would be: using a `workspace = true` source in development, then a `path` source to a specific wheel in production. See: https://github.com/astral-sh/uv/issues/9675.

---

_Label `question` removed by @charliermarsh on 2024-12-06 11:59_

---

_Label `enhancement` added by @charliermarsh on 2024-12-06 11:59_

---

_Label `needs-design` added by @charliermarsh on 2024-12-06 11:59_

---

_Comment by @charliermarsh on 2024-12-08 04:29_

It doesn't solve the broader "prod vs. dev" problem, but I think we should allow conflicting URLs between `project.dependencies` and `tool.uv.sources`. It's legitimately useful and semantically reasonable (and costs us nothing).

---

_Comment by @edmorley on 2025-01-27 14:45_

Would something similar to how Cargo's `[patch]` or paths override features are implemented work here?
https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html#the-patch-section
https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html#paths-overrides

Specifically, Cargo allows defining those settings via config files outside of the project (eg a global machine config) allowing one to override the `Cargo.toml` committed to Git, and thus change how dependencies are sourced locally.

---

_Comment by @arkadyark-cohere on 2025-04-07 22:14_

Is there any recommended way to support dev / prod installations in the case where we want to use a published package from a private artifact registry (e.g. GCP)? In that case, I believe the `--no-sources` trick won't work, as sources are needed to specify both versions (editable and artifact registry).

---

_Comment by @konstin on 2025-04-08 08:51_

How are you installing the editable? Usually, using an editable through `tool.uv.sources` should work, and by having a version in `project.dependencies`, `--no-sources` should also do.

---

_Comment by @arkadyark-cohere on 2025-04-08 16:09_

If I'm using an artifact registry, then I believe in order to specify both the editable version, and the index version, my pyproject.toml needs to contain something like:

```
[[tool.uv.index]]
name = "registry"
url = "link-to-gcp-artifact-registry"

[tool.uv.sources]
dep = { path = "../dep", editable = true }
dep = { index = "registry" }
```

this fails when I try to `uv lock` with duplicate entries in sources - is there a correct way to do this?

---

_Comment by @konstin on 2025-04-08 17:05_

That's indeed a missing feature, specifically that index pinning does not work with `--no-sources`.

---

_Comment by @mshunshin on 2025-06-26 01:17_

The issue with using --no-sources is that you must then use it for every incantation of uv run, else you run into this bug #7572

---

_Comment by @Laz4rz on 2025-08-04 10:14_

When specifying source in dependencies and then specifying separate source in uv.sources — as to have a prod/dev uv sync --no-sources/uv sync separation -- uv doesn't correctly resolve dependencies of the `library`.

```
dependencies = [
   "library @ git+ssh://git@github.com/library/library-core.git@feature/next",
]
```

```
[tool.uv.sources]
 library= { path = "../library", editable = true }
```

above fails to resolve dependencies for library

---

_Comment by @charliermarsh on 2025-08-04 11:14_

Please create a separate issue with a minimal reproduction that we can run ourselves.

---

_Comment by @waynew on 2025-08-09 19:05_

FWIW, I don't think it works *particularly* well (there are some vagaries when it comes to `uv lock`)  but 

```
[project]
dependencies = [
    "some-pkg>=1.2.3",
]

[tool.uv.sources]
some-pkg =  { path = "../ww.hello_world", editable=true}

[dependency-groups]
dev = [
    "some-pkg",
]
```

Worked for me, somewhat. Like, I was able to do a uv install and make changes with `some-pkg` and see them live in my project, and then I was able to build & install just the wheel in a different environment. I can probably do a writeup of my exact steps if it would help anyone, and the process felt super brittle... but I was able to at least get the behavior that I wanted 🤷 

---

_Comment by @centaurialpha on 2025-08-09 19:20_

@waynew It would be very helpful if you shared the steps, I try to do something like that in my job a long time ago. It is the only thing that stops us from migrating completely to uv

---

_Comment by @waynew on 2025-08-10 23:55_

I wrote up here: https://waynewerner.com/blag/2025-08-09-14-43-editable-dev-dependencies-with-uv.html

Looks like basically `uv add` and `uv add --dev --editable /some/dependency` and then you have to install with the wheels with the actual deployment.

---

_Comment by @vitschwilk on 2025-11-10 16:09_

I think having a configurable source would be a huge benefit. E.g. when we integrate our module we don't want to deploy multiple dev version to our private pypi repository but want to use editable installs. But in a pipeline scope the paths might differ from a dev environment or might even be referenced form a different repo. 

e.g.

```toml
dev=[ #default if no sources specified (?)
my_other_module = { path = "../my_other_module/othermodule", editable = false }
]

local=[
my_other_module = { path = "../my_other_module/othermodule", editable = true }
]

integration=[
my_other_module = { git = "github_repo_uri", subdirectory = "othermodule", branch = "integration" }
]
```

Currently the pipeline fails until the modules that are to be integrated are deployed. But sometimes we need to work on multiple modules in parallel for the integration.

Syncing with `uv sync sources=integration` would then provide only the sources in "integration" when calling `uv sync` would be the same as calling `uv sync sources=dev`

The workaround form @waynew did not work for my use case since `uv sync` always uses `dependency-groups.dev`. My use case for `dependency-groups.dev` is something like "unittest" or "coverage", which are always need on a developer machine and on the pipeline. Also I would think running integration tests on a pipeline should include installing the correct version locked version of the dependency overwriting the version in a dev-dependency-group with any version can result is a broken dependency in the deployed version. 

tl:dr: I think being able to specify which sources (group) to use would be very useful for a pipeline use case. I would highly appreciate this feature since it would make uv complete for me.

---
