```yaml
number: 6722
title: "`uv sync --no-install-package` without pyproject.toml"
type: issue
state: open
author: sbidoul
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-08-27T21:13:41Z
updated_at: 2025-09-25T14:37:40Z
url: https://github.com/astral-sh/uv/issues/6722
synced_at: 2026-01-12T15:59:06Z
```

# `uv sync --no-install-package` without pyproject.toml

---

_@sbidoul_

On the same theme as #6685 and #6573, but easier to solve?

Since the package name is provided on the CLI and also present in uv.lock, there should be no need to read pyproject.toml at all?

---

_Comment by @charliermarsh on 2024-08-27 21:15_

> Since the package name is provided on the CLI...

Do you mean, if you use `uv sync --package`?

---

_Comment by @zanieb on 2024-08-27 21:15_

Similar to #6573, part of the complexity is that we use the `pyproject.toml` to determine the root directory of the workspace. We'd need to use the `uv.lock` to encode this instead, just in this case?

---

_Comment by @zanieb on 2024-08-27 21:15_

I think the thought is `--no-install-package my-root-package` (or any other package)

---

_Comment by @charliermarsh on 2024-08-27 21:16_

Ohh I see. I think I'd rather that we put the workspace root in `uv.lock`, and then allow `uv sync` or `uv sync --package`.

---

_Comment by @charliermarsh on 2024-08-27 21:20_

Honestly it would be easy to support `uv sync --package`, and users could pass the workspace root, but I know that's a little tedious.

---

_Comment by @sbidoul on 2024-08-27 21:29_

To clarify, my use case is still the dependencies-only layer of a Dockerfile.
It sill itches me a little bit to have to copy pyproject.toml in addition to uv.lock.

`uv sync --no-install-package my-root-package --frozen` seemed quite close to it: as you mentioned in https://github.com/astral-sh/uv/issues/6573#issuecomment-2308381467 that pyproject.toml is used only to obtain the package name, I assumed it was not necessary since in the case of `--no-install-package`, the package name is known upfront from the CLI.





---

_Comment by @charliermarsh on 2024-08-27 23:18_

Yeah it bothers me too. I think we can fix it holistically.

---

_Comment by @charliermarsh on 2024-08-27 23:47_

I guess one challenging case is: you're workspace is rooted at `root`, and you're at `root/packages/foo`. Normally, if you ran `uv sync`, we'd sync `foo`. But if we don't have any `pyproject.toml` etc., we have no way of knowing that you're in that workspace member. So `uv sync --frozen` would have to sync the workspace root instead.

---

_Comment by @charliermarsh on 2024-08-28 02:00_

The other strange piece is that you lose any configuration defined in `pyproject.toml`. (Just documenting things I notice as I work on this.)

---

_Label `enhancement` added by @charliermarsh on 2024-08-28 02:25_

---

_Comment by @sbidoul on 2024-08-28 03:33_

Ah, and extras may also influence the design. What if, for instance, I want to seed the dependencies of my root package but only some of it's optional dependencies?

---

_Comment by @zanieb on 2024-08-28 12:38_

@charliermarsh that seems particularly problematic :/ we don't really want to copy all those into the lock do we?

---

_Comment by @charliermarsh on 2024-08-28 12:59_

> Ah, and extras may also influence the design. What if, for instance, I want to seed the dependencies of my root package but only some of it's optional dependencies?

This would work as expected, I think, since we store this information in the lock.

> that seems particularly problematic :/ we don't really want to copy all those into the lock do we?

No, I don't think so. The situation we're at now is we still require the root `pyproject.toml` but none of the members (as of my PR last night).

I think we _can_ support not requiring the `pyproject.toml` at all, but users would need to copy it over if they had configuration defined in there.


---

_Label `needs-decision` added by @zanieb on 2024-09-15 22:00_

---

_Comment by @legraphista on 2024-10-22 15:38_

To add to @sbidoul usecase:

> To clarify, my use case is still the dependencies-only layer of a Dockerfile. It sill itches me a little bit to have to copy pyproject.toml in addition to uv.lock.

Because we need to copy the `pyproject.toml` over alongside the `uv.lock` in the Dockerfile
```Dockerfile
COPY ./pyproject.toml ./pyproject.toml
COPY ./uv.lock ./uv.lock
RUN uv sync --no-install-package --no-cache --frozen --no-dev
```
every time we update the version number, the previous cached layer containing the dependencies is invalidated, even though `uv.lock` hasn't changed. 

This is not that big deal for projects that are light on dependencies, but for projects that use ML workloads, re-installing and duplicating ~5GBs of dependencies every time the version changes is wasteful.

___ 

For anyone looking for a solution in the meantime, export the dependencies to `requirement.txt` via `uv export` and use that.

```shell
echo "--extra-index-url https://{...}/simple" > requirements.txt
uv export --format requirements-txt --no-hashes --no-editable --no-dev >> requirements.txt
```
```Dockerfile
COPY ./requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r ./requirements.txt
```

---

_Comment by @sobolevn on 2025-01-14 12:46_

The solution provided by @legraphista works, but it is a bit of a hack. It also requires to add this action on `pre-commit` / build step.

That's one of the missing features that stops me from addopting `uv` (along side with `dependabot` support).

---

_Comment by @danielgafni on 2025-02-04 16:22_

I can suggest a different solution which works `uv sync` natively:

1. Set `project.dynamic = ["version"]`  (extend to include [other fields](https://hatch.pypa.io/1.12/config/metadata/#configuring-project-metadata) if needed):

```toml
[project]
dynamic = ["version"]
```

2. Configure your build system (it's defined in `[build-system]`) to dynamically fill the value for this field instead of hard-coding it in `pyproject.toml`. [Here](https://hatch.pypa.io/1.12/version/#configuration) is how this can be achieved with Hatch (`uv`'s default build system). 
```toml
[tool.hatch.version]
path = "src/your_lib/_version.py"
```

This `_version.py` file can contain any logic for setting the `version` variable (for example, reading it from an environment variable). 

---

This will decouple `pyproject.toml` from the ever changing `version` value. 

---

_Comment by @sobolevn on 2025-02-04 16:36_

@danielgafni yes, this works for `version`, but it does not work for tools that also store configuration in `pyproject.toml` like `ruff` / `black` / etc. You can create custom files for all tools, but it requires a different layout. 

---

_Comment by @danielgafni on 2025-02-04 17:04_

Oh right. I mean some tools have their own config files (like `ruff.toml`) too, but conceptually that's still a problem. 

---

_Comment by @twmht on 2025-04-13 03:09_

> To add to [@sbidoul](https://github.com/sbidoul) usecase:
> 
> > To clarify, my use case is still the dependencies-only layer of a Dockerfile. It sill itches me a little bit to have to copy pyproject.toml in addition to uv.lock.
> 
> Because we need to copy the `pyproject.toml` over alongside the `uv.lock` in the Dockerfile
> 
> COPY ./pyproject.toml ./pyproject.toml
> COPY ./uv.lock ./uv.lock
> RUN uv sync --no-install-package --no-cache --frozen --no-dev
> every time we update the version number, the previous cached layer containing the dependencies is invalidated, even though `uv.lock` hasn't changed.
> 
> This is not that big deal for projects that are light on dependencies, but for projects that use ML workloads, re-installing and duplicating ~5GBs of dependencies every time the version changes is wasteful.
> 
> For anyone looking for a solution in the meantime, export the dependencies to `requirement.txt` via `uv export` and use that.
> 
> echo "--extra-index-url https://{...}/simple" > requirements.txt
> uv export --format requirements-txt --no-hashes --no-editable --no-dev >> requirements.txt
> COPY ./requirements.txt ./requirements.txt
> RUN pip install --no-cache-dir -r ./requirements.txt

I'm currently encountering the same problem. The version numbers in my pyproject.toml are being updated within the CI/CD pipeline. This leads to cache misses and increases the size of my Docker image with each build. Is there a way to make uv sync --frozen --no-install-project only look at uv.lock and ignore pyproject.toml?

---

_Comment by @charliermarsh on 2025-04-14 12:27_

Yeah we do want to support this. It's entirely possible, it just requires some refactoring.

---

_Comment by @jaens on 2025-05-09 16:53_

It's possible to do this fully in Docker right now with a bit of duct tape, because Docker itself can also run `uv export`.

Example:

```dockerfile
FROM ... AS base
# ... base image commands

FROM base AS uv-freeze

COPY pyproject.toml uv.lock .
RUN uv export --format requirements-txt --no-hashes --no-editable --no-dev > requirements.txt && \
  touch -t 202001010000 requirements.txt
  # Docker hates reproducible builds, I think

FROM base AS uv-install

COPY --from=uv-freeze requirements.txt .
RUN uv venv && uv pip install --requirements=requirements.txt

# proceed with your venv...
# (either directly or if you need this workaround, probably copying it into yet another stage)
```

Might be nice to wrap this into a prettier package though.

---

_Comment by @osemenovsky on 2025-09-24 15:13_

Hello, is this being worked on?
We have the same problem with the docker build cache being invalidated every time we change the version in pyproject.toml. Granted, uv is very fast so it's not a huge problem as far as build time goes, but it does take registry space and we'd like to be able to use uv without the hacks provided in this thread.
So far I'm on the fence between just generating a pyproject.toml file specifically to install dependencies and using the requirements.txt export method, but i'd rather not have to do any of those things.

EDIT: temporarily solved the issue by generating a dummy pyproject.toml instead of COPYing it:

```Dockerfile
RUN cat > pyproject.toml <<EOF
[project]
name = 'project-name'
dynamic = ['version']
EOF

COPY uv.lock ./
ENV UV_PROJECT_ENVIRONMENT=/usr/local
RUN uv sync --frozen --inexact
```

---
