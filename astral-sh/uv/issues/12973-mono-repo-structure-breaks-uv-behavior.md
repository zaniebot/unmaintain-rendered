---
number: 12973
title: mono repo structure breaks uv behavior
type: issue
state: open
author: kotoroshinoto
labels:
  - bug
assignees: []
created_at: 2025-04-18T17:39:46Z
updated_at: 2025-04-21T19:06:55Z
url: https://github.com/astral-sh/uv/issues/12973
synced_at: 2026-01-10T01:25:27Z
---

# mono repo structure breaks uv behavior

---

_Issue opened by @kotoroshinoto on 2025-04-18 17:39_

### Summary

Our repo has a structure like this one: https://github.com/kotoroshinoto/ExampleMultiRepo

Uv insists on resolving paths relative to the repo root and ignores everything I have tried to point it to the actual top level pyproject.toml

It's a corporate project. I have to share this repo with non-python projects. They don't want a top level pyproject.tonl.

Please give us some kind of mechanism to force uv to use the location of the toml as the root like it should.

### Platform

windows11

### Version

0.6.14

### Python version

3.11

---

_Label `bug` added by @kotoroshinoto on 2025-04-18 17:39_

---

_Comment by @charliermarsh on 2025-04-21 01:56_

I don't fully understand the issue, your report is pretty sparse, but: (1) `${PROJECT_ROOT}` always resolve to the current working directory, it doesn't do anything special beyond that -- I'm guessing you're running your commands from the repo root, and that's causing you confusion? (See https://github.com/astral-sh/uv/issues/3271, which would change that behavior).

And (2) I really think you want `[tool.uv.sources]` instead of `${PROJECT_ROOT}` which is largely a legacy thing. Specifically, instead of:

```toml
[project.optional-dependencies]
dev = [
    "example-project-package0 @ file:///${PROJECT_ROOT}/packages/package0",
    "example-project-package1 @ file:///${PROJECT_ROOT}/packages/package1",
    "example-project-package2 @ file:///${PROJECT_ROOT}/packages/package2",
]
```

You want:

```toml
[project.optional-dependencies]
dev = [
    "example-project-package0 @ file:///${PROJECT_ROOT}/packages/package0",
    "example-project-package1 @ file:///${PROJECT_ROOT}/packages/package1",
    "example-project-package2 @ file:///${PROJECT_ROOT}/packages/package2",
]

[tool.uv.sources]
example-project-package0 = { path = "packages/package0" }
...
```

That allows you to use relative paths.

---

_Comment by @kotoroshinoto on 2025-04-21 18:54_

At first I was trying to use pdm's experimental support to use it as a package resolver 

I expanded testing to the uv commands directly, which is where I started to see the issue. It would alternately try to find the sub package directly under project root (dropping the packages directory from the path like {Project_root}/package0) if the command was executed from the directory containing the outermost project.toml or if run from inside packages directory, that part of the path was doubled. {Project_root}/packages/packages/package0.

If setting the source paths using [tool.uv.sources] resolves thisI will let you know. Ideally it shouldn't be maiming the paths like that even if the sources aren't set though.

---

_Comment by @kotoroshinoto on 2025-04-21 19:03_

Additionally this is a mono repo. The packages aren't really supposed to be added as includes/sources in the external toml.

The packages and package# directories will not be part of the import paths for the subprojects. The top level imports will be directories contained by the sub projects pointed to in their own toml configuration file.

---
