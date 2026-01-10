---
number: 10591
title: "Use versions from requirements.txt during `uv lock`"
type: issue
state: closed
author: dionhaefner
labels:
  - question
assignees: []
created_at: 2025-01-14T12:05:23Z
updated_at: 2025-01-16T03:47:44Z
url: https://github.com/astral-sh/uv/issues/10591
synced_at: 2026-01-10T01:24:55Z
---

# Use versions from requirements.txt during `uv lock`

---

_Issue opened by @dionhaefner on 2025-01-14 12:05_

Our current workflow looks like this:
- All direct dependencies are pinned in `requirements.txt` for reproducible installations (used during testing on CI and deployment).
- Dependencies in `pyproject.toml` are unconstrained, so dev installations (via `pip install -e .`) always get the latest versions of all dependencies.
- We have set up dependabot to bump versions in `requirements.txt` once per week to detect problems with new library releases and update deployment versions.

How can we replicate the workflow above with uv, using lock files during CI and deployment for full reproducibility?

Since dependabot doesn't support uv lock files, I thought we'd likely want to keep `requirements.txt` as single-source truth for tried and tested versions of direct dependencies, then generate a lock file from that. But there seems to be no convenient way to go from `requirements.txt` -> `uv.lock`?

Grateful for any input, including alternative solutions to implement something similar to what we currently have and general best practices.

---

_Comment by @charliermarsh on 2025-01-14 13:51_

Hmm, you would generally go the other way, and generate a `requirements.txt` from a `uv.lock` with `uv export`. I think what you could do is:

- Leave dependencies unconstrained in `pyproject.toml`.
- Put the direct dependency pins in `tool.uv.constraint-dependencies` (in `pyproject.toml`).
- Run `uv lock`, then `uv export`.

But yeah, you'll lose some of the convenience of Dependabot until uv is supported.


---

_Label `question` added by @charliermarsh on 2025-01-14 13:51_

---

_Comment by @dionhaefner on 2025-01-14 19:28_

Thanks for weighing in @charliermarsh. How do people typically implement similar patterns in a `uv`-native way? Would something like this make sense?

1. Ditch the `requirements.txt` + dependabot entirely and just keep dependencies in `pyproject.toml` with loose constraints (for example, just a lenient lower bound) to get flexible dev setups.
2. Keep a `uv.lock` under version control that represents a fully reproducible environment that's known to be working (used for tests on CI and deployment).
3. Add a cron task that runs weekly and executes `uv sync` to bump `uv.lock`.
4. If there's an incompatibility discovered that way, exclude it from the supported versions in `pyproject.toml`.
5. ~~Add `uv.lock` to `.gitignore` so devs don't accidentally push their local versions.~~
6. Optionally, also run `uv export` after (3) to create a `requirements.txt` for non-`uv` deployment.

---

_Comment by @zanieb on 2025-01-14 19:33_

Until Dependabot support lands, you could switch to Renovate? We've been pretty happy with it here 🤷‍♀ 

re. (3) You could run `uv lock --upgrade` to discover incompatible new versions, unless you are running tests against the versions (in which case you'll want `uv sync --upgrade`).
re. (5) I wouldn't add it to the `.gitignore`. The lockfile should be stable unless explicitly updated. In the Cargo/Rust ecosystem, it's not normal to need to ignore the `Cargo.lock` file.

---

_Comment by @dionhaefner on 2025-01-14 19:38_

Thanks, `uv sync --upgrade` is what I had in mind for (3) :)

Good pointer on (5), I guess the missing piece for me was that `uv sync` alone doesn't modify the lock file even if there have been new releases of deps in the meantime.

---

_Comment by @dionhaefner on 2025-01-14 19:40_

Regarding (4) – would we exclude those versions from `[project.dependencies]` or from `[tool.uv.constraint-dependencies]`? I'm guessing the former if they are direct dependencies and the latter otherwise?

---

_Comment by @zanieb on 2025-01-14 19:55_

Yes that sounds correct re (4).

---

_Comment by @dionhaefner on 2025-01-15 15:31_

Seems like that's going to work. Thanks all!

---

_Closed by @dionhaefner on 2025-01-15 15:31_

---

_Comment by @truonghm on 2025-01-16 03:47_

Hi all, I have a similar question but with a different need: we were using uv but will the pip-tools interface (i.e. uv pip compile and uv pip sync) via a `requirements.txt` / `requirements-dev.txt` combination. We would like to move to using uv.lock instead, however we want to keep all package versions the same as in `requirements.txt`. I have try running `uv lock` and some of the packages would be upgraded as expected since those packages are unconstrained in `pyproject.toml`.

I figure this might be a common use case for people looking to move from older uv versions to newer ones with a more uv-native way of package management but want to keep it safe regarding package versions. Appreciate any suggestions for this, thank you!

---
