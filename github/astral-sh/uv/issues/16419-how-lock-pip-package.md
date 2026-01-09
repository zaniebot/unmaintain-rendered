---
number: 16419
title: how lock pip package ?
type: issue
state: open
author: balabalabutton
labels:
  - question
assignees: []
created_at: 2025-10-23T14:26:02Z
updated_at: 2025-12-18T16:15:20Z
url: https://github.com/astral-sh/uv/issues/16419
synced_at: 2026-01-07T13:12:19-06:00
---

# how lock pip package ?

---

_Issue opened by @balabalabutton on 2025-10-23 14:26_

### Question

I want to lock the versions of some packages so that they will never be updated. Even if `uv add` is used, it will be blocked directly.

In actual use (or maybe I used it wrong?), `uv lock` only updates the lock file, and it seems that it cannot lock the package version.

I found a way to do it, but it's awkward. Here are the steps:

```shell
cd ~/project-a
uv export --no-dev --format requirements.txt -o consistent.txt

cd ~/project-b
uv add pandas -c consistent.txt
```
This step is awkward, and it's easy to forget to add the subsequent constraint behavior.


I also tested the following steps：
```shell
uv add numpy==2.2.2
uv lock

uv add numpy==2.3.3 --frozen
```
Although it is shown as 2.2.2 in uv.lock, it is 2.3.3 in the pyproject.toml file and the output of `print(numpy.__version__)` is the same

Am I not using uv correctly to fully restrict the package version?



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @balabalabutton on 2025-10-23 14:26_

---

_Comment by @balabalabutton on 2025-10-23 14:27_

Or maybe I just mistakenly thought that uv can achieve limited package version?

---

_Comment by @konstin on 2025-10-23 15:50_

For the package interface (`uv sync`, `uv add`, `uv lock`), you can use constraint dependencies (https://docs.astral.sh/uv/reference/settings/#constraint-dependencies). Why do you want to exclude certain versions?

---

_Comment by @balabalabutton on 2025-10-23 18:14_

> For the package interface (`uv sync`, `uv add`, `uv lock`), you can use constraint dependencies (https://docs.astral.sh/uv/reference/settings/#constraint-dependencies). Why do you want to exclude certain versions?对于包接口（`uv sync`、`uv add`、`uv lock`），可以使用约束依赖项 （https://docs.astral.sh/uv/reference/settings/#constraint-dependencies）。为什么要排除某些版本？

Although not tested, it seems that you can set `uv init --config-file` to initialize the project and overwrite pyproject.toml.

This does not mean that all packages must strictly control their versions. It is just for the convenience of unified project management. Just like Python versions need to be controlled, core packages such as numpy and sklearn need to be kept consistent.

In this way, the core package is put into the base image, and then the core package is restricted by `constraint-dependencies` so that it will not be upgraded by mistake.

Otherwise, overly open package usage can cause problems for different but related projects. At least building images after management can reduce time waste, duplicate layers, and redundant storage in Harbor.

By the way, can `uv init project --config-file uv.toml` render the configuration information in the file into the project's `pyproject.toml`? I still need to verify it.

---

_Comment by @balabalabutton on 2025-10-24 02:00_

here is uv.toml
```toml
constraint-dependencies = ["grpcio<1.65"]
```
and run:
```shell
uv add "grpcio==1.65" --config-file uv.toml
Resolved 2 packages in 601ms
warning: `grpcio==1.65.0` is yanked (reason: "https://github.com/grpc/grpc/issues/37178")
Prepared 1 package in 10.71s
Installed 1 package in 3ms
 + grpcio==1.65.0
```
There seems to be no limit to success

After writing the contents of uv.toml into pyproject.toml, the following error is reported:
```shell
uv add "grpcio==1.66"                      
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The following fields from `[tool.uv]` will be ignored in favor of the `uv.toml` file:
- constraint-dependencies
  × No solution found when resolving dependencies:
  ╰─▶ Because your project depends on grpcio==1.66 and grpcio<1.65, we can conclude that your project's requirements are
      unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and
        syncing.

rm uv.toml

uv add "grpcio==1.66"
  × No solution found when resolving dependencies:
  ╰─▶ Because your project depends on grpcio==1.66 and grpcio<1.65, we can conclude that your project's requirements are
      unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and
        syncing.
```
`uv init project --config-file uv.toml` will not append the content to the project's `pyproject.toml`，It seems that this can only be achieved by manually copying the content into the project after creating it.

---

_Comment by @konstin on 2025-12-18 16:15_

We generally recommend that projects use requirement bounds that reflect the versions they support, we don't support loading general constraints as configuration beyond `--constraints` and constraint-dependencies.

---
