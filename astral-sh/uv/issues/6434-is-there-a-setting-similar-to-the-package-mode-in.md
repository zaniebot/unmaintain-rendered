```yaml
number: 6434
title: "Is there a setting similar to the `package-mode` in poetry?"
type: issue
state: closed
author: pythonweb2
labels: []
assignees: []
created_at: 2024-08-22T13:37:47Z
updated_at: 2024-08-28T18:08:42Z
url: https://github.com/astral-sh/uv/issues/6434
synced_at: 2026-01-12T15:59:04Z
```

# Is there a setting similar to the `package-mode` in poetry?

---

_@pythonweb2_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv version: 0.3.1
uv platform: linux

Poetry supports what they call operating modes (https://python-poetry.org/docs/basic-usage/#operating-modes), where if you are not planning on publishing your project, you can specify `package-mode = false`, which only installs the dependencies listed, but does not install the project into the virtual environment.

I was unable to find something like this mentioned in the docs for projects, is it unsupported or perhaps missing from the docs (or perhaps documented somewhere I couldn't see it).

---

_Comment by @zanieb on 2024-08-22 15:55_

We don't support this yet, but there's discussion about excluding the project from installation in various issues.

What's your use-case for this? Why does it matter if the project is installed?

---

_Comment by @z3z1ma on 2024-08-22 23:29_

This is a blocker, or at the very least annoying. I can workaround with a shim src dir that just has an empty init. 

Lots of people may need to manage a bunch of python dependencies outside the context of having a top-level package they themselves are trying to install. Hence why poetry created the package-mode=false. It lets everyone benefit from the lockfile, resolver, and toolchain. A simple example is https://github.com/apache/airflow which has huge adoption and use at large companies -- but there are obv tons of cases where you need to manage libraries that (may) operate in a context independent of a structured python package. 

---

_Comment by @charliermarsh on 2024-08-22 23:33_

Just to clarify, are you citing Airflow as an example of a project that is not structured as a package? Or an example of something you might use outside of a package?

---

_Comment by @z3z1ma on 2024-08-22 23:46_

The latter.

---

_Comment by @z3z1ma on 2024-08-22 23:52_

More random data-oriented examples @charliermarsh 

https://github.com/dbt-labs/dbt-core
https://github.com/TobikoData/sqlmesh
https://github.com/feast-dev/feast
https://github.com/mlflow/mlflow
https://github.com/dagster-io/dagster
https://github.com/PrefectHQ/prefect
https://github.com/dlt-hub/dlt
https://github.com/meltano/meltano


We can imagine all the AI / ML / data pipeline / automation  code (disparate python scripts in a repository, NOT a src application) we'd like to have lockfiles and uv tooling for. 

EDIT: for clarity, these are all things a developer may depend on in a repo that is not otherwise structured as a python application itself.

---

_Comment by @zanieb on 2024-08-22 23:57_

It sounds like you're looking for a virtual project, which I believe we support if you enumerate everything as a development dependency? As described in https://docs.astral.sh/uv/concepts/workspaces/#workspace-roots

---

_Comment by @z3z1ma on 2024-08-23 00:08_

No, as far as I can tell (and I just tried it) you still need a pyproject.toml somewhere in the repo with `project` defined. If not in the top level explicit root, then in the member(s) of the virtual root?

In all my examples, dbt or sqlmesh being really straightforward ones, you will not have a `src` directory or anything to install. But still want to manage a graph of dependencies for the repo to operate.

---

_Comment by @zanieb on 2024-08-23 00:11_

Worked fine for me? Did you encounter an error?

```
❯ uv init --virtual
Initialized workspace `example`
❯ uv add --dev httpx
Using Python 3.11.7
Creating virtualenv at: .venv
Resolved 7 packages in 268ms
Prepared 7 packages in 61ms
Installed 7 packages in 13ms
 + anyio==4.4.0
 + certifi==2024.7.4
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.0
 + idna==3.7
 + sniffio==1.3.1
❯ uv run python -c "import httpx"
❯ tree .
.
├── README.md
├── pyproject.toml
└── uv.lock

1 directory, 3 files
❯ cat pyproject.toml
[tool.uv]
dev-dependencies = [
    "httpx>=0.27.0",
]
[tool.uv.workspace]
members = []
```

---

_Comment by @pythonweb2 on 2024-08-23 00:16_

My use case in particular is a Django application.

In some cases, it may be desired to package a django app, but in most cases, the root project will never be packaged, however a cross platform lockfile is very useful for CI pipelines, and a better workflow than pip in my opinion.

It looks like the virtual project may work, I will take a look into this.

---

_Comment by @z3z1ma on 2024-08-23 00:17_

NVM I see, workspace members can be empty. 

I am working with an existing project at work and trying to adapt it to `uv` so trying to grok everything from the docs. While they mention the notion of virtual root, it doesn't really describe in an obvious way that its a way to manage deps for a project which itself has no installable package. Its focused around `pyproject.toml`s in subdirectories which are themselves valid `project`s

That worked for me!

Q I suppose. Does this give us a single layer of dependencies? Whereas with the `project.dependencies` and `project.optional-dependencies` we would get to delineate combinations of deps. 
`poetry` supports this in the pending PR for PEP 621 support which works fine with package-mode false (which is to say it just doesnt look for or install the package declared by `project.name` AFAICT. 

---

_Comment by @z3z1ma on 2024-08-23 00:22_

I'm going to go out on a limb and say, while we can potentially achieve our goal (creating a cross-platform lockfile from pyproject.toml without a `src` package) with virtual root and empty members -- it seems obtuse if you zoom out. Like an indirect path. It's solely my gut-feel so its fine to push back. 

BTW really excited for `uv` and thanks for all the work, today has been my breaking it in day so pardon finding and banging on this issue. But from a data / ML engineer perspective -- this is an extremely common scenario. :) 

---

_Comment by @charliermarsh on 2024-08-23 00:24_

Appreciate the kind words :)

Personally I still feel we can do better with what we're calling "virtual" projects. I'm not a big fan of using virtual workspaces to solve this problem. But I'm mostly focusing on correctness issues in the immediate term so we need a bit of time to step back and think about where to improve.

---

_Comment by @charliermarsh on 2024-08-23 14:53_

Wrote up an idea here: https://github.com/astral-sh/uv/issues/6511

---

_Comment by @charliermarsh on 2024-08-28 18:08_

This exists now as of [v0.4.0](https://github.com/astral-sh/uv/releases/tag/0.4.0)!

Projects that omit `[build-system]` will no longer be built and installed by default. You can also set `tool.uv.package` to `false` to control the behavior explicitly, as in:

```toml
[tool.uv]
package = false
```

---

_Closed by @charliermarsh on 2024-08-28 18:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 18:08_

---
