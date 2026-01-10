```yaml
number: 7487
title: "dev-dependencies in workspace pyproject.toml not installed with `uv run`"
type: issue
state: closed
author: KilianMichiels
labels:
  - question
assignees: []
created_at: 2024-09-18T08:36:09Z
updated_at: 2025-10-30T14:37:53Z
url: https://github.com/astral-sh/uv/issues/7487
synced_at: 2026-01-10T03:23:52Z
```

# dev-dependencies in workspace pyproject.toml not installed with `uv run`

---

_Issue opened by @KilianMichiels on 2024-09-18 08:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We have a mono repository with a couple of packages and services. I wanted to unify the dev-dependencies for all packages (each of them have their own pyproject.toml file) since they are largely the same dependencies.

I noticed when moving the `dev-dependencies` from the package pyproject.toml files to the workspace one in the root folder, that these dependencies are no longer installed when I run `uv run` in a package folder (e.g., to run a test script with pytest).

Is this behaviour expected? I assumed dependency resolution would take into account all `dev-dependencies` in all pyproject.toml files inside the workspace.


---

_Comment by @charliermarsh on 2024-09-18 14:23_

To confirm my understanding: you want to define the `dev-dependencies` in a root `pyproject.toml`, and then have them available when running commands like `uv run --package some-member` -- is that right?

---

_Label `question` added by @charliermarsh on 2024-09-18 14:24_

---

_Comment by @KilianMichiels on 2024-09-18 14:44_

Hi @charliermarsh, that is correct! Since I'm working in a workspace where things like pytest, coverage, ruff, etc. are all shared, it seemed like the logical thing to do.

I added an example below to explain my use case a bit further:

Directory structure:
```
<root>
  -- pyproject.toml
  -- scripts
  -- other_stuff
  -- my-package
      -- pyproject.toml
      -- my-package
      -- tests
```

In my root directory, I have this `pyproject.toml`:
```
[project]
name = "my-project"
version = "0.1.0"
requires-python = ">=3.10,<3.13"
description = "My simple project"

[tool.uv]
# NOTE: These dependencies are not picked up when running
#       in a package folder. Therefore I kept them in the
#       pyproject.toml of each package for now.
dev-dependencies = [
    "bandit>=1.7.9",
    "pytest-cov>=5.0.0",
    "pytest-env>=1.1.3",
    "pytest-xdist>=3.6.1",
    "pytest>=8.3.2",
    "ruff>=0.6.5",
]

[tool.uv.sources]
my-package = { workspace = true }

[tool.uv.workspace]
members = [
    "my-package",
]
```

Then in my package folder I have this `pyproject.toml`:

```
[project]
name = "my-package"
version = "0.1.0"
requires-python = ">=3.10,<3.13"
description = "My simple package"

[tool.uv]
# I would expect these to not be needed anymore as they are defined in the dev-dependencies of the workspace.
dev-dependencies = [
    "bandit>=1.7.9",
    "pytest-cov>=5.0.0",
    "pytest-env>=1.1.3",
    "pytest-xdist>=3.6.1",
    "pytest>=8.3.2",
    "ruff>=0.6.5",
]
```

Then I'd like to be able to do for example:
```
cd my-package
uv run ruff check my-package/
```

And this gives me the error (on a cold run, so no `uv sync` was run yet, inside a Docker Gitlab CI) if I comment out the `dev-dependencies` in the package `pyproject.toml` and only have the ones in the root file:
```
> error: Failed to spawn: `ruff`
  >   Caused by: No such file or directory (os error 2)
```

---

Additionally, it might be nice to be able to add common dependencies to the root workspace `pyproject.toml` and add more specific ones to the package `pyproject.toml`.

**Edit:** Added `my-package = { workspace = true }` and `members` to the root `pyproject.toml` to better represent my current implementation.

---

_Renamed from "dev-dependencies in workspace pyproject.toml not installed with `uv sync`" to "dev-dependencies in workspace pyproject.toml not installed with `uv run`" by @KilianMichiels on 2024-09-18 14:46_

---

_Comment by @BurntSushi on 2024-09-18 15:21_

I think I like the status quo, where you can do `dep = { workspace = true }` to inherit the specification for `dep`, so you don't need to repeat the version constraints, but still requires opt-in to add the dev-dependency in the first place. I wouldn't expect all packages in all workspaces to need the same dev-dependencies in all cases, so if we made this case work as expected, I'd also expect some way to opt out of it. Another approach here might be to add an option for dev-dependencies as to whether they should be "forcefully" inherited in packages contained in the workspace.

---

_Comment by @KilianMichiels on 2024-09-18 16:39_

> I think I like the status quo, where you can do dep = { workspace = true } to inherit the specification for dep, so you don't need to repeat the version constraints, but still requires opt-in to add the dev-dependency in the first place. 

Is this the functionality under `[tool.uv.sources]` you are talking about? As I thought this solution was mainly to set your own packages as part of the workspace environment?  I tried adding `ruff = {workspace = true}` in my root `pyproject.toml` but got a `Package is not included as workspace package in 'tool.uv.workspace'` error...which makes sense.

> I wouldn't expect all packages in all workspaces to need the same dev-dependencies in all cases, so if we made this case work as expected, I'd also expect some way to opt out of it.

I agree this is not by default the case. I believe the proposed solution should indeed be an opt-in if you add specific dev-dependencies to your root `pyproject.toml`. Then only those would be shared across the workspace. Not doing so, essentially results in the current working implementation where only dev-dependencies in the package's `pyproject.toml` are picked up. But since I'm quite new to `uv` I might be missing something here. :)

> Another approach here might be to add an option for dev-dependencies as to whether they should be "forcefully" inherited in packages contained in the workspace.

I don't think I have enough experience with `uv` to provide any useful input here, but here's my 2 cents :) On the one hand this would make the settings more explicit which is nice. On the other hand, it might add more complexity to the `pyproject.toml` files and one might lose the overview of which packages force this behaviour if it should be set in the package's `pyproject.toml`. If it's a single setting in the root `pyproject.toml` I think it could be nice, but I don't see the added benefit of having both the setting _and_ common dev-dependencies (adding them to the root implies having this setting on `true` as far as I can see).

---

_Comment by @BurntSushi on 2024-09-18 16:54_

The issue is when you add a `dev-dependency` to the root but _don't_ want it to be included in a sub-package. That's how dev-dependencies and normal dependencies work today. Correct me if I'm wrong, but I believe you are suggesting that dev-dependencies should work differently: that a dev-dependency in the workspace root `pyproject.toml` should be automatically inherited by all workspace packages.

The `{ workspace = true }` thing applies to your `my-package` example, not the workspace root. It's saying, "use the value defined in the workspace root `pyproject.toml`."

---

_Comment by @KilianMichiels on 2024-09-19 10:54_

Oh now I understand! In that case, my suggestion indeed works differently than the current behavior. So this might be quite a big change to add and I don't think I have a proper overview of the impact of such a change..

I'm only looking for a way to only define some dev-dependencies once, if they are shared across all packages / services / ... in the workspace. Like `ruff`! :) But as far as I can see, there is no easy way to do this? The only solution I could find was adding `ruff` to all my dev-dependencies for all `pyproject.toml` files. If there is a better way, that would be more than welcome! :)

---

_Comment by @BurntSushi on 2024-09-19 12:01_

So one thing it looks like I got wrong is that `{ workspace = true }` is not working as I had expected it to. From the docs:

> The `workspace = true` key-value pair in the tool.uv.sources table indicates the bird-feeder dependency should be provided by the workspace, rather than fetched from PyPI or another registry.

That's saying the dependency should be _in_ the workspace. I had gotten this wrong and thought it meant that it "inherits" what dependency specification is in the workspace root. So I think the situation is a little worse than I was painting: you do have to repeat not just the dependency but the version constraint.

I don't have a strong opinion about whether there's an option to make dev-dependencies apply everywhere, but I do kind of think it should be opt-in. Or maybe even opt-in on a dependency-by-dependency basis in the workspace root.

---

_Comment by @KilianMichiels on 2024-09-19 12:56_

I really like the idea of having the opt-in on a dependency-by-dependency basis in the workspace root! However, looking at the [docs](https://docs.astral.sh/uv/concepts/dependencies/#development-dependencies) I don't see a ready-to-use solution that aligns neatly with PEP 508 and allows you to set a flag or something for each dev-dependency? So perhaps a global opt-in under `[tool.uv]` or `[tool.uv.dev-dependencies]` would already be an easy win until there is a need for a more fine-grained solution? 

---

_Closed by @charliermarsh on 2024-12-26 16:00_

---

_Comment by @alex-debrecht-kcftech on 2025-01-22 01:33_

I see this is closed as completed, but I can't find anything in the documentation about enabling functionality like this, and trying `uv run --exact --package ...` does not install either `dev-dependencies` or the `dev` dependency group from the workspace root. Am I missing something, or should this be closed as "not planned"?

---

_Closed by @charliermarsh on 2025-01-22 01:37_

---

_Comment by @charliermarsh on 2025-01-22 01:37_

I'll re-close as "not planned". No, we don't allow you to define dependencies in the workspace root that apply to individual members.

---

_Comment by @chuikoffru on 2025-01-28 12:05_

`uv sync --all-extras`

---

_Comment by @anentropic on 2025-08-12 18:27_

I have the opposite problem

I want to be able to easily, from the project root, install the dev dependencies for project workspaces when they are defined in the workspaces rather than the project root

`uv sync --all-extras` does not do this AFAICT

I mostly want this so that VS Code can just point to the venv of the project root and not have missing packages when editing files in any of the workspaces

---

_Comment by @anentropic on 2025-08-12 18:29_

ah ok, `uv sync --all-packages` does what I need

as mentioned on https://github.com/astral-sh/uv/issues/7541#issuecomment-2487051795 

---

_Comment by @nfantone on 2025-10-02 19:20_

Apologies for reviving this, but today I hit the same wall.

As someone with a more JS-centric background, where a root `package.json` would [typically define cross-package/common dependencies](https://github.com/vercel/next.js/blob/canary/package.json#L115) and configuration, I find the pattern very natural. From reading this thread, I'm not sure I gather why this was rejected.

@charliermarsh 

> No, we don't allow you to define dependencies in the workspace root that **apply to individual members**.

Not sure this is the right mental framework for the feature being requested here, if I may. These wouldn't be dependencies applying to "individual members", but to _all_ members, indiscriminately. As @KilianMichiels elaborated in his original post, classic examples of this are testing (`pytest`), linting (`ruff`) and type checking (`mypy`). As your project grows, you wouldn't want to repeat these deps every time and deal with the bookkeeping that comes with it. 

In every case, their configuration is already resolved by traversing the file system upwards — which means that running `pytest` or `ruff` from inside an individual package would fetch settings from the root. The convenience of this flow is deterred by how `uv sync` and `uv run` currently operate.

Given the following monorepo setup:

```txt
py-project/
├── pyproject.toml           # Shared dev tools and config (mypy, ruff, pytest)
├── uv.lock                  
├── packages/
│   ├── my-package/
│   │   ├── pyproject.toml   # Package specific deps
│   │   └── src/...
│   └── my-service/
│       ├── pyproject.toml
│       └── myservice/...
```

My current workaround for running tools inside a package dir is:


```sh
cd packages/my-package

# Install dev deps from the root
uv sync --only-dev --no-install-workspace --directory ../../ 

# Add package-specific dependencies
uv sync --inexact

# Run package only tests
uv run pytest
```

Which feels redundant, but does the trick. `--all-extras` doesn't help here and `--all-packages` would install dependencies from unrelated packages.

---

_Comment by @anentropic on 2025-10-03 08:08_

I agree that being able to specify some dev dependencies in the root project would be useful

I realise there is a tension between whether root project deps should belong "to the root project itself" as currently, or "to all workspaces"

But in a monorepo scenario the root project is often an empty stub that just contains the workspaces, and the latter pattern would be more useful. Perhaps it could be opt-in

(I encountered recently that when building to publish there is already a behaviour that supports this, i.e. `uv build` at the root builds the root itself, while `uv build --all-packages` at the root builds only the workspaces and not the root)

---

_Comment by @wxgeo on 2025-10-30 14:37_

Sad it was rejected.
Having to duplicate dev-dependencies 4 times and maintaining all of them synchronized manually seems like a non-sense to me.
I'm switching from poetry to uv, mainly because I was expecting a lot from uv workspaces, but apart from the common `uv.lock`, I don't see much benefice from using them.


---
