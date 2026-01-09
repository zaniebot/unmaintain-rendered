---
number: 8334
title: "Run `uv sync` in a workspace member as if no workspace root was present"
type: issue
state: open
author: thorbenk
labels: []
assignees: []
created_at: 2024-10-18T15:03:08Z
updated_at: 2025-04-16T18:18:44Z
url: https://github.com/astral-sh/uv/issues/8334
synced_at: 2026-01-07T13:12:17-06:00
---

# Run `uv sync` in a workspace member as if no workspace root was present

---

_Issue opened by @thorbenk on 2024-10-18 15:03_

I have the following setup:

- A workspace root, declaring multiple workspace members. This is for development convenience (can hack on all interdependent packages at once)
- The workspace members, each git submodules, are all independently published and have their independent CI.
  In particular, they all have their own `uv.lock` files.

Now, if I want to pass the CI for workspace member "package_a", I would like to do:

```
cd packages/workspace_a
uv --no-discover-workspace sync
uv --no-discover-workspace run pytest .
```

which I would expect would add a `.venv` in `packages/workspace_a` as well as a `uv.lock` file. The dependency calculation would all be done without knowing that this package is actually part of a workspace, and the test would run exactly as in that independent CI.

Thanks for `uv` and for considering my use-case :sweat_smile: 

---

_Comment by @AirswitchAsa on 2024-12-20 15:47_

I am in a very similar situation as you, where my company wants to organize multiple services written in python in one workspace but at the same time being able to build and deploy them as if they are not part of a workspace. I think #9571 is also related.

It seems you may do `uv run --package` if all you want is an isolated environment locally to run `pytest`. In my case, I would still need the individual lock files to build the container-based service from the service root instead of workspace root. And it can be a plus if we can do that from root with something like `uv lock --all-packages` to generate individual lock files, whether as a subset of the root lock file or individually locked.

---

_Comment by @wsascha on 2025-02-04 20:42_

It'd be a great feature to lock workspace member packages.

### Use case

For developing an application package, some libraries may be included as submodules into that application package. These libraries should have their own lock files for managing them separately and we may wish to update those also during application package development. However, this is not possible when the library packages are inside of a workspace.

### Proposal

Looking at

```sh
uv run --package ...
```

sth similar for locking could look like

```sh
uv lock --package ...
```

### MWE

```bash
uv init foo
cd foo

mkdir packages
cd packages
uv init bar
cd bar

uv add pydantic # updates /foo/packages/bar/pyproject.toml & /foo/uv.lock
uv lock # nothing happens, /foo/uv.lock is already up-to-date

# Comment out 'members = ["packages/bar"]' in /foo/pyproject.toml
sed --in-place --expression='/members = /s/^/# /g' ../../pyproject.toml
uv lock # updates /foo/packages/bar/uv.lock
```




---

_Comment by @jamestwebber on 2025-04-16 15:55_

I'm looking into `uv` workspaces for our projects and running into some similar questions. These projects are currently set up as individual repos on Github with their own CI/CD and github action. They depend on each other with tagged git dependencies (i.e. `dep = { git = "https://github.com/.../dep", rev = "0.1.0" }`.

I would _like_ to create a workspace where we can develop these projects in a coordinated fashion, but it's unclear how to do that without a substantial refactor of our tools (which is very unlikely). I'm not sure submodules would help or hurt us in this case--we'd prefer that Github actions continue to run in the individual repositories, when applicable.

One possibility I thought of was to set up a workspace but actually `.gitignore` the individual repositories inside. Using the example from docs:

```
albatross
├── packages
│   ├── bird-feeder  # from github.com/org/bird-feeder, but this directory is in top-level .gitignore
│   │   ├── pyproject.toml
│   │   ├── uv.lock  # is this allowed?
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds # from github.com/org/seeds, also ignored
│       ├── pyproject.toml
│       ├── uv.lock
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── albatross
        └── main.py
```

But I'm not sure if those inner `uv.lock` files will function properly

---

_Comment by @lagamura on 2025-04-16 18:18_

@jamestwebber 

>These projects are currently set up as individual repos on Github with their own CI/CD and github action.

As we are working with the same structure - multiple repos, I found useful when developing in the main repo which depends on our packages, to use the:
```toml
[tool.uv.sources]
package_1 = [{ path = "../package_1" }]
package_2 = [{ index = "../package_2" }]
...
```
You still need to `uv sync` when you change your packages, but seems the best approach so far. I admit I haven't try submodules in a big monorepo mirror with all the packages.

---

_Referenced in [astral-sh/uv#12984](../../astral-sh/uv/issues/12984.md) on 2025-04-20 03:20_

---
