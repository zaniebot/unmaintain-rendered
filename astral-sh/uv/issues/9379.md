```yaml
number: 9379
title: Frozen vs Locked unexpected behavior
type: issue
state: closed
author: TurtleOrangina
labels:
  - question
assignees: []
created_at: 2024-11-23T09:54:59Z
updated_at: 2024-11-23T17:06:01Z
url: https://github.com/astral-sh/uv/issues/9379
synced_at: 2026-01-10T04:36:20Z
```

# Frozen vs Locked unexpected behavior

---

_Issue opened by @TurtleOrangina on 2024-11-23 09:54_

Hi,

in my Dockerfile I do:

```
# Install python dependencies (only main, not dev dependencies)
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-dev
```

This works fine, and installs only the dependencies, not the project itself. But, I would like to check if the uv.lock file is actually up-to-date with the pyproject.toml file. So I added `--locked`. This doesn't work due to:
`error: the argument '--frozen' cannot be used with '--locked'`. So I remove --frozen, but that seems to change something with regards to build behavior, because I no longer manage to pass this step in my Docker container, I get:

```
#16 [builder  8/10] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --locked --no-install-project --no-dev
#16 0.089 Using CPython 3.13.0 interpreter at: /usr/local/bin/python3
#16 0.089 Creating virtual environment at: /venv
#16 0.493 error: Failed to generate package metadata for `workoutsummary==0.3.0 @ editable+.`
#16 0.493   Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_editable` (exit status: 1)
#16 0.493 
#16 0.493 [stdout]
#16 0.493 Checking for Rust toolchain....
#16 0.493 Running `maturin pep517 write-dist-info --metadata-directory /root/.cache/uv/builds-v0/.tmpVutI4o/metadata_directory --interpreter /root/.cache/uv/builds-v0/.tmpVutI4o/bin/python`
#16 0.493 
#16 0.493 [stderr]
#16 0.493 💥 maturin failed
#16 0.493   Caused by: Can't find /app/Cargo.toml (in /app)
#16 0.493 Error running maturin: Command '['maturin', 'pep517', 'write-dist-info', '--metadata-directory', '/root/.cache/uv/builds-v0/.tmpVutI4o/metadata_directory', '--interpreter', '/root/.cache/uv/builds-v0/.tmpVutI4o/bin/python']' returned non-zero exit status 1.
#16 ERROR: process "/bin/sh -c uv sync --locked --no-install-project --no-dev" did not complete successfully: exit code: 2
------
 > [builder  8/10] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --locked --no-install-project --no-dev:
0.493   Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_editable` (exit status: 1)
0.493 
0.493 [stdout]
0.493 Checking for Rust toolchain....
0.493 Running `maturin pep517 write-dist-info --metadata-directory /root/.cache/uv/builds-v0/.tmpVutI4o/metadata_directory --interpreter /root/.cache/uv/builds-v0/.tmpVutI4o/bin/python`
0.493 
0.493 [stderr]
0.493 💥 maturin failed
0.493   Caused by: Can't find /app/Cargo.toml (in /app)
0.493 Error running maturin: Command '['maturin', 'pep517', 'write-dist-info', '--metadata-directory', '/root/.cache/uv/builds-v0/.tmpVutI4o/metadata_directory', '--interpreter', '/root/.cache/uv/builds-v0/.tmpVutI4o/bin/python']' returned non-zero exit status 1.
------
Dockerfile:41
--------------------
  40 |     # Install python dependencies (only main, not dev dependencies)
  41 | >>> RUN --mount=type=cache,target=/root/.cache/uv \
  42 | >>>     --mount=type=bind,source=uv.lock,target=uv.lock \
  43 | >>>     --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
  44 | >>>     uv sync --locked --no-install-project --no-dev
  45 |     
--------------------
ERROR: failed to solve: process "/bin/sh -c uv sync --locked --no-install-project --no-dev" did not complete successfully: exit code: 2
```

As context, this is a mixed python+rust project, with `maturin` as build-backend. The unexpected behavior here is that maturin is called in a way that demands a `Cargo.toml` file when I use `--locked` vs `--frozen`.

What I would have wished for:
- `--locked` and `--frozen` being non exclusive, as frozen seems to be more than just the instruction to not write the uv.lock file.
- Alternatively, if `--frozen` really only would skip the writing of the `uv.lock` file, I would expect the above error to not occur when I substitute `--frozen` with `--locked`.

---

_Comment by @TurtleOrangina on 2024-11-23 09:56_

If required, I can create a minimal example to reproduce. Just let me know if that would be useful.

---

_Comment by @FishAlchemist on 2024-11-23 10:13_

Isn't using ``--locked`` sufficient to check if uv.lock is up-to-date?
```bash
uv sync --locked
```
If you simply want the lock file to be automatically updated to the latest version, then you don't need to use either --locked or --frozen
> The project is re-locked before syncing unless the --locked or --frozen flag is provided.

By using both ``--locked`` and ``--frozen``, are you attempting to make UV verify that the lock file is current without allowing it to modify the lock file? 


https://docs.astral.sh/uv/reference/cli/#uv-sync
``--locked``
```
Assert that the uv.lock will remain unchanged.

Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.

May also be set with the UV_LOCKED environment variable.
```
``--frozen``
```
Sync without updating the uv.lock file.

Instead of checking if the lockfile is up-to-date, uses the versions in the lockfile as the source of truth. If the lockfile is missing, uv will exit with an error. If the pyproject.toml includes changes to dependencies that have not been included in the lockfile yet, they will not be present in the environment.

May also be set with the UV_FROZEN environment variable.
```


---

_Comment by @charliermarsh on 2024-11-23 13:02_

`--locked` requires that we can access the project metadata. Are you using dynamic metadata, or something like that? If so, we have to build the project from source.

---

_Label `question` added by @charliermarsh on 2024-11-23 13:02_

---

_Comment by @TurtleOrangina on 2024-11-23 16:55_

> `--locked` requires that we can access the project metadata. Are you using dynamic metadata, or something like that? If so, we have to build the project from source.

Indeed, my `pyproject.toml` contained `dynamic = ["version"]` - I am not sure why. I don't think it was necessary, as the version itself was a fixed string. Removing this line makes it work with `--locked` instead of `--frozen`.

I have also made a example that reproduces it, in case it is useful:
https://github.com/Lingepumpe/uv_locked_frozen_reproduction

Simply running `docker build .` in the repo will result in the error - commenting out the dynamic line in pyproject.toml will avoid the error.

The question remains: Why does `--locked` require extra steps when compared with `--frozen`? In the scenario of the Dockerfile, the idea is to check the uv.lock file and install the dependencies as early as possible, without needing to copy in the full source tree, for optimal layer caching. 

---

_Comment by @charliermarsh on 2024-11-23 17:06_

> The question remains: Why does --locked require extra steps when compared with --frozen?

Because `--locked` has to validate that the lockfile is up-to-date, which requires accessing the project's metadata, which in your case requires building the project. `--frozen` doesn't have to validate anything, so it doesn't need to build the metadata, since you're not installing the project.

---

_Closed by @charliermarsh on 2024-11-23 17:06_

---
