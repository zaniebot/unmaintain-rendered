```yaml
number: 11048
title: "Entrypoint script shebang uses resolved path for symlinked Python rather than `sys.executable`"
type: issue
state: closed
author: edmorley
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-01-29T01:22:53Z
updated_at: 2025-01-31T20:03:37Z
url: https://github.com/astral-sh/uv/issues/11048
synced_at: 2026-01-12T16:00:26Z
```

# Entrypoint script shebang uses resolved path for symlinked Python rather than `sys.executable`

---

_@edmorley_

### Summary

The shebang lines of entrypoint scripts created by `uv sync` when using `UV_PROJECT_ENVIRONMENT` with a symlinked Python installation use the resolved Python binary path rather than the `sys.executable` path, which differs from pip and `uv pip`'s behaviour and causes some issues with the older of our two build systems.

### Steps

1. Create a `Dockerfile` based on the below.
2. `docker build . --progress plain --no-cache`

(We don't actually use Docker to build on our older build system - the below is for MRE purposes only)

```Dockerfile
FROM heroku/heroku:24-build AS build
ARG TARGETARCH
USER root

COPY --from=ghcr.io/astral-sh/uv:0.5.25 /uv /usr/local/bin

WORKDIR /tmp/build
COPY <<EOF pyproject.toml
[project]
name = "example"
version = "0.0.0"
requires-python = ">=3.13"
dependencies = ["gunicorn"]
EOF

RUN mkdir -p /tmp/build/.heroku/python \
  && curl -f "https://heroku-buildpack-python.s3.us-east-1.amazonaws.com/python-3.13.1-ubuntu-24.04-${TARGETARCH}.tar.zst" \
  | tar -x --zstd -C /tmp/build/.heroku/python
RUN mkdir -p /app/.heroku \
  && ln -s /tmp/build/.heroku/python /app/.heroku/python
ENV PATH="/app/.heroku/python/bin:${PATH}"
ENV LD_LIBRARY_PATH="/app/.heroku/python/lib"

# These all output paths under `/app/.heroku/python` rather than `/tmp/...`:
RUN which python && which python3
RUN python -c "import sys; print(sys.prefix); print(sys.executable)"

# However, when using `uv sync` with `UV_PROJECT_ENVIRONMENT` set, the gunicorn
# shebang is the resolved Python location under /tmp rather than the path under /app:
# `#!/tmp/build/.heroku/python/bin/python3`
ENV UV_PROJECT_ENVIRONMENT="/app/.heroku/python"
RUN RUST_LOG="uv=debug,uv_python=trace" uv sync
RUN head -n1 $(which gunicorn)

# In the run-time environment, the app is mounted at /app, which means the gunicorn
# entrypoint will only work if it has a shebang using the /app path.
FROM heroku/heroku:24
COPY --from=build --chown=heroku /tmp/build /app
ENV PATH="/app/.heroku/python/bin:${PATH}"
ENV LD_LIBRARY_PATH="/app/.heroku/python/lib"
RUN gunicorn --version
```

### Expected

- For the various `which ...` commands, plus `sys.path` and `sys.executable` to all report paths under `/app/...` (rather than `/tmp/...`).
- For the gunicorn shebang to be `#!/app/.heroku/python/bin/python`
- For the `gunicorn --version` command to succeed.

### Actual

- The various `which ...` commands, plus `sys.path` and `sys.executable` all report paths under `/app/...`.
- However, the `head -n1 $(which gunicorn)` command shows the gunicorn entrypoint as having a shebang of `#!/tmp/build/.heroku/python/bin/python3`
- The `gunicorn --version` command fails due to the shebang referencing the wrong path (`/tmp/...`, which no longer exists).

See:

<details>

<summary>Docker build output</summary>

```
$ docker build . --progress plain --no-cache
...
#15 [build  7/10] RUN which python && which python3
#15 0.128 /app/.heroku/python/bin/python
#15 0.128 /app/.heroku/python/bin/python3
#15 DONE 0.1s

#16 [build  8/10] RUN python -c "import sys; print(sys.prefix); print(sys.executable)"
#16 0.086 /app/.heroku/python
#16 0.086 /app/.heroku/python/bin/python
#16 DONE 0.1s

#17 [build  9/10] RUN RUST_LOG="uv=debug,uv_python=trace" uv sync
#17 0.131 DEBUG uv 0.5.25
#17 0.132 DEBUG Found project root: `/tmp/build`
#17 0.132 DEBUG No workspace root found, using project root
#17 0.133 DEBUG Using Python request `>=3.13` from `requires-python` metadata
#17 0.133 TRACE Querying interpreter executable at /tmp/build/.heroku/python/bin/python3
#17 0.158 DEBUG The virtual environment's Python version satisfies `>=3.13`
#17 0.159 DEBUG Using request timeout of 30s
#17 0.160 DEBUG Found static `pyproject.toml` for: example @ file:///tmp/build
#17 0.160 DEBUG No workspace root found, using project root
#17 0.162 DEBUG Solving with installed Python version: 3.13.1
#17 0.162 DEBUG Solving with target Python version: >=3.13
#17 0.163 DEBUG Adding direct dependency: example*
#17 0.163 DEBUG Searching for a compatible version of example @ file:///tmp/build (*)
#17 0.163 DEBUG Adding direct dependency: gunicorn*
#17 0.164 DEBUG No cache entry for: https://pypi.org/simple/gunicorn/
#17 0.197 DEBUG Searching for a compatible version of gunicorn (*)
#17 0.198 DEBUG Selecting: gunicorn==23.0.0 [compatible] (gunicorn-23.0.0-py3-none-any.whl)
#17 0.198 DEBUG No cache entry for: https://files.pythonhosted.org/packages/cb/7d/6dac2a6e1eba33ee43f318edbed4ff29151a49b5d37f080aad1e6469bca4/gunicorn-23.0.0-py3-none-any.whl.metadata
#17 0.242 DEBUG Adding transitive dependency for gunicorn==23.0.0: packaging*
#17 0.242 DEBUG No cache entry for: https://pypi.org/simple/packaging/
#17 0.250 DEBUG Searching for a compatible version of packaging (*)
#17 0.250 DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
#17 0.250 DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
#17 0.261 DEBUG Tried 3 versions: example 1, gunicorn 1, packaging 1
#17 0.261 DEBUG all marker environments resolution took 0.099s
#17 0.262 Resolved 3 packages in 103ms
#17 0.264 DEBUG Using request timeout of 30s
#17 0.265 DEBUG Identified uncached distribution: gunicorn==23.0.0
#17 0.265 DEBUG Identified uncached distribution: packaging==24.2
#17 0.265 DEBUG No cache entry for: https://files.pythonhosted.org/packages/cb/7d/6dac2a6e1eba33ee43f318edbed4ff29151a49b5d37f080aad1e6469bca4/gunicorn-23.0.0-py3-none-any.whl
#17 0.265 DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl
#17 0.316 Prepared 2 packages in 51ms
#17 0.331 Installed 2 packages in 14ms
#17 0.331  + gunicorn==23.0.0
#17 0.331  + packaging==24.2
#17 DONE 0.3s

#18 [build 10/10] RUN head -n1 $(which gunicorn)
#18 0.114 #!/tmp/build/.heroku/python/bin/python3
#18 DONE 0.1s

#19 [stage-1 2/3] COPY --from=build --chown=heroku /tmp/build /app
#19 DONE 0.1s

#20 [stage-1 3/3] RUN gunicorn --version
#20 0.075 /bin/sh: 1: gunicorn: not found
#20 ERROR: process "/bin/sh -c gunicorn --version" did not complete successfully: exit code: 127
```

</details>

### Other findings

For comparison, if I use `uv pip install --system` instead, then I get the shebang I expect. e.g: Try replacing the three `uv sync` related lines above with:

```Dockerfile
RUN uv pip install --system .
RUN head -n1 $(which gunicorn)
```

Similarly, if that's then replaced with `pip install`, the shebang is also as expected:

```Dockerfile
RUN python -m ensurepip --default-pip
RUN python -m pip install .
RUN head -n1 $(which gunicorn)
```

Interestingly, `uv venv` does use `/app` paths for the `pyvenv.cfg` metadata and the symlink target:

```Dockerfile
RUN uv venv /example-venv
RUN grep home /example-venv/pyvenv.cfg
RUN readlink /example-venv/bin/python
```

```
#20 [build 12/13] RUN grep home /example-venv/pyvenv.cfg
#20 0.085 home = /app/.heroku/python/bin
#20 DONE 0.1s

#21 [build 13/13] RUN readlink /example-venv/bin/python
#21 0.120 /app/.heroku/python/bin/python3
#21 DONE 0.1s
```

So I'm presuming this might be specific to when `UV_PROJECT_ENVIRONMENT` is used?

I also tried setting `UV_PYTHON` explicitly (both to `/app/.heroku/python` and `/app/.heroku/python/bin/python`), but that didn't affect the shebang.

### Related issues

These look vaguely related?
- https://github.com/astral-sh/uv/issues/9739
- https://github.com/astral-sh/uv/issues/9046

### Context

Heroku has two platform generations, each with its own build system and buildpack implementations.

In the older generation (Cedar, which uses classic buildpacks), the app source is located at a location like `/tmp/build_<hash>` during the build, then at app run-time the app archive is mounted at `/app`. [1]

As you can imagine for Python this relocation can cause some issues, however, we're able to work around these pretty easily in practice, by:
1. At build time, creating a temporary symlink [2] from `/app/.heroku/python` (which is what will be the actual location at app run-time) to `/tmp/build_<hash>/.heroku/python` (the location of Python install during the build)
2. Adding `/app/.heroku/python/bin` to `PATH` instead of the `/tmp` location
3. After the build, performing rewriting of any `.pth` files created for editable local directory installs

This works because:
- Python will resolve it's location relative to the path it was invoked from (which was `/app/.heroku/python/bin/python` given that's what's on `PATH`), and thus `sys.prefix` / `sys.executable` use `/app/...` paths
- Package managers typically use `sys.executable` when calculating the shebangs for entrypoint scripts, eg:
  - pip uses `sys.executable` [here](https://github.com/pypa/pip/blob/f5ff4fa27dd991f209f2a9f9283a7c1561221b2c/src/pip/_internal/operations/install/wheel.py#L105C19-L105C33)
  - Poetry uses the [installer](https://github.com/pypa/installer) package, which uses `sys.executable` via [here](https://github.com/pypa/installer/blob/4c9fd28ddcf30f2a5e9a269c3aaa0517e862b809/src/installer/__main__.py#L106) and [here](https://github.com/pypa/installer/blob/4c9fd28ddcf30f2a5e9a269c3aaa0517e862b809/src/installer/utils.py#L189)

...and thus entrypoint scripts will end up with a shebang like:
`#!/app/.heroku/python/bin/python`

...which will also work at app run-time, when the app is actually mounted at `/app`.


[1] The original need to use a different directory has long since passed, however, it's not possible for us to change the build location without breaking many third-party classic buildpacks, so we're sadly stuck with this relocation for the classic build system. Thankfully the [Cloud Native Buildpack](https://buildpacks.io/) spec has the build time and run time paths the same, so our newer build system doesn't have to work around this issue.

[2] The reason for the symlink and not copying the app source back and forth from /tmp <-> /app at the start/end of the Python build step is due to a combination of compatibility concerns with other language buildpacks (that can run before/after the Python buildpack and can also use our Python install), plus build end to end time impact from the additional I/O.


### Platform

Ubuntu (but not using distro Python)

### Version

0.5.25

### Python version

Python 3.13.1

---

_Label `bug` added by @edmorley on 2025-01-29 01:22_

---

_Label `great writeup` added by @zanieb on 2025-01-29 02:14_

---

_Assigned to @zanieb by @zanieb on 2025-01-29 02:14_

---

_Comment by @zanieb on 2025-01-29 02:15_

Thanks for all the details! I'll dig into this tomorrow.

---

_Comment by @zanieb on 2025-01-29 02:30_

Briefly...

> Python will resolve it's location relative to the path it was invoked from (which was /app/.heroku/python/bin/python given that's what's on PATH), and thus sys.prefix / sys.executable use /app/... paths

When you use `UV_PROJECT_ENVIRONMENT` we don't find the interpreter on the PATH, we discover it in the directory you provide. It looks like somewhere we're resolving a symbolic link, because in the logs you can see us query the interpreter at the `/tmp` location:

> TRACE Querying interpreter executable at /tmp/build/.heroku/python/bin/python3

Given your above statement (thank you so much for sharing that information!) I believe this would cause the behavior you're seeing.

It's relatively unambiguous that we use `sys.executable` during installs

https://github.com/astral-sh/uv/blob/ad075c375187fdaeba25c1272e3c7a4275810ce4/crates/uv-install-wheel/src/wheel.rs#L203

---

_Comment by @zanieb on 2025-01-29 20:00_

And it looks like canonicalization happens at https://github.com/astral-sh/uv/blob/bec8468183c7cc1697ad1d34a5eb6087ec5c8a90/crates/uv-python/src/environment.rs#L164

---

_Comment by @zanieb on 2025-01-29 20:40_

Interesting. I fixed the canonicalization there, but... we still get the same value for `sys.executable` — perhaps I'm missing something? https://github.com/astral-sh/uv/pull/11083

edit: Oh interesting, it's different in CI than locally! https://github.com/astral-sh/uv/pull/11083#issuecomment-2622819665

---

_Closed by @zanieb on 2025-01-30 16:10_

---

_Comment by @edmorley on 2025-01-31 19:51_

I can confirm this is resolved with #11083 - my tests now pass - thank you!

---

_Comment by @zanieb on 2025-01-31 20:03_

Thanks for following up Ed!

---
