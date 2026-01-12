```yaml
number: 5561
title: Add constraints (and overrides?) to build environments
type: issue
state: closed
author: charliermarsh
labels:
  - help wanted
  - compatibility
assignees: []
created_at: 2024-07-29T14:24:42Z
updated_at: 2024-09-01T16:57:23Z
url: https://github.com/astral-sh/uv/issues/5561
synced_at: 2026-01-12T15:58:57Z
```

# Add constraints (and overrides?) to build environments

---

_@charliermarsh_

We should add a `--build-constraints` that apply to the build environments.

See: https://github.com/astral-sh/uv/issues/5551#issuecomment-2256055975

---

_Label `compatibility` added by @charliermarsh on 2024-07-29 14:24_

---

_Comment by @hauntsaninja on 2024-07-29 18:27_

(Note that pip's behaviour is a little weird, only the env var affects the build environments, not the command line flag)

---

_Comment by @charliermarsh on 2024-07-29 19:54_

Oh wow, that's interesting. I'm considering providing separate API for this (like `--build-constraints`).

---

_Label `help wanted` added by @charliermarsh on 2024-07-29 21:31_

---

_Comment by @orsinium on 2024-07-30 07:00_

In [DepHell](https://github.com/dephell/dephell), I used to use [ensurepip](https://docs.python.org/3/library/ensurepip.html) with [fallbacks](https://github.com/dephell/dephell_venvs/blob/master/dephell_venvs/_ensurepip.py) to bootstrap build venvs. It uses the setuptools and pip versions bundled with stdlib that are always the same and guaranteed to be stable.

---

_Closed by @charliermarsh on 2024-08-02 02:15_

---

_Closed by @charliermarsh on 2024-08-02 02:15_

---

_Comment by @grant-zietsman on 2024-09-01 13:00_

@charliermarsh Are there any plans to make `--build-constraints` a project setting (similar to [constraint-dependencies](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies))?

Currently build constraints need to be manually specified:
- `--build-constraint` (seems exclusive to the pip interface).
- `UV_BUILD_CONSTRAINT` (seems global).

Having a central project setting would be convenient (at least for the use case I have in mind).

### Use Case (Context)

The goal is for packages (source distributions) that include protobufs to support compiling these protobufs during build while being constrained to the same major version of protobuf as required by the project into which they are being installed.

Constraining the version of `protobuf` both during build and via [constraint-dependencies](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies) (in the end project) _should_ ensure compatibility, if these packages (source distributions) compile protobufs with `grpcio-tools` during build.

This should also be attainable with [no-build-isolation](https://docs.astral.sh/uv/reference/settings/#no-build-isolation) and [no-build-isolation-package](https://docs.astral.sh/uv/reference/settings/#no-build-isolation-package) but it  would be preferred to retain build isolation (to avoid unnecessarily constraining any other build requirements).

A workaround that somewhat side steps this is to compile protobufs on import.

---

_Comment by @charliermarsh on 2024-09-01 16:56_

Yeah we should add this. I'll make a separate issue.

---

_Comment by @charliermarsh on 2024-09-01 16:57_

Tracking here: https://github.com/astral-sh/uv/issues/6913

---
