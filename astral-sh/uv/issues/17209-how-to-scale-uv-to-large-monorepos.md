---
number: 17209
title: How to scale uv to large monorepos?
type: issue
state: open
author: danijar
labels:
  - question
assignees: []
created_at: 2025-12-21T21:09:49Z
updated_at: 2025-12-28T22:09:02Z
url: https://github.com/astral-sh/uv/issues/17209
synced_at: 2026-01-10T01:26:15Z
---

# How to scale uv to large monorepos?

---

_Issue opened by @danijar on 2025-12-21 21:09_

## Question

We have a large monorepo and would love to use native uv but are running into some current limitations. Mainly, we would like to use a shared namespace folder structure and separate lock files and venvs for each package. I've opened this thread to discuss which potential new features would be required for uv to scale to large monorepos while avoiding common issues.

## Example

Shared folder structure for all projects (`pyproject.toml` files on the "inside"):

```
packages
  org
    project
      models
        foo
          pyproject.toml
          uv.lock
          .venv
          __init__.py
          train.py
        bar
          pyproject.toml
        baz
          pyproject.toml
      datasets
        foo
          pyproject.toml
        bar
          pyproject.toml
        baz
          pyproject.toml
...
```

Imports include the namespaces (either relative or the absolute):

```python3
# org/project/models/foo/train.py
from org.project.datasets import foo, bar, baz
from ..datasets import foo, bar, baz
```

## Rationale

### Shared namespace folder structure

A flat list of packages is hard to maintain at scale: it would be hundreds of folders and each would have its namespace inlined into the folder name with dashes. Moreover, currently each package would have to duplicate its nested namespace subtree inside. That's too many folders to keep in sync and navigate between. A shared namespace folder structure scales well to any number of packages.

### Separate lock files and venvs for each package

Forcing all teams to the same version lock file means leads to frequent and unexpected breakages for other teams. Moreover, the more packages use the same dependency, the harder it becomes to ever update that dependency, leading to version lock-in. Separate lock files and venvs for each package allow developers to predictably upgrade at their own convenience, which scales well to any number of packages.

## Open problems

### pyproject.toml inside namespace leaf folders

Placing `pyproject.toml` files inside the leafs of the namespace folder structure seems currently not supported. In principle, it's easy to support by a build backend. I've wrote a small working build backend as an example here: https://gist.github.com/danijar/755c7dbef9ac0819081b26818245c73b

### Path dependencies relative to repo root

With its single lock file and venv, it seems like uv workspaces only scale to small monorepos, so we should probably go with separate projects with path dependencies. Ideally, it would be possible to specify path dependencies relative to the repo root. We could patch that functionality on top of uv, for which I've opened an issue here: https://github.com/astral-sh/uv/issues/17247

### Building all wheels needed to deploy one package

Deployments would require building a subtree of wheel files, similar to `uv build --all` in a uv workspace, but limited to only the dependency subtree of a specific project for faster builds and lightweight deployments. I've opened an issue here: https://github.com/astral-sh/uv/issues/17248

---

_Label `question` added by @danijar on 2025-12-21 21:09_

---

_Referenced in [astral-sh/uv#17154](../../astral-sh/uv/issues/17154.md) on 2025-12-21 21:18_

---

_Renamed from "Does uv allow a shared namespace structure for separate projects (with separate lock files)?" to "How to scale uv to large monorepos?" by @danijar on 2025-12-28 20:45_

---

_Referenced in [astral-sh/uv#17283](../../astral-sh/uv/issues/17283.md) on 2026-01-02 16:32_

---
