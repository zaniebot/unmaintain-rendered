```yaml
number: 16428
title: "`uv python find` does not detect venv Python in CPython 3.13t"
type: issue
state: closed
author: lmmx
labels:
  - bug
assignees: []
created_at: 2025-10-24T01:25:43Z
updated_at: 2025-10-24T09:32:59Z
url: https://github.com/astral-sh/uv/issues/16428
synced_at: 2026-01-12T16:02:31Z
```

# `uv python find` does not detect venv Python in CPython 3.13t

---

_@lmmx_

### Summary

I tried removing all aspects of this and could not find any confounding factors so I think it's a real bug.

I isolated a bug using a CI workflow that appears to show CPython 3.13t, 3.14t not picking up the venv in the way expected via the `uv python find` API after `uv sync` in an immediately prior step in the same job

- **edit** originally I thought this was other versions, later clarified only 3.13t, 3.14t

I echoed the python it found and it was `/usr/bin/python3`

My understanding it is best practice to use `uv python find` over just directly referencing `.venv/bin/python`

- I used v6 of the `astral-sh/setup-uv` Action here but I then bumped to v7 and it didn't heal so it's not that
- I removed Swatinem/rust-cache and the mold linker and didn't heal so not that
- The error said maturin not found but I could call maturin in `.venv/bin` directly so not that
- I turned off the `with_cache` in the setup uv action step but not that
- I tried passing `--directory .venv` to hint to look in there but same as without so not that

CI run link: https://github.com/lmmx/comrak/actions/runs/18766098367/job/53541371018?pr=28

<details>
<summary>📝 Click to show CI logs</summary>

```

Run uv sync --only-group build --only-group test
  uv sync --only-group build --only-group test
  shell: /usr/bin/bash -e {0}
  env:
    UV_PYTHON: 3.13t
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
    CARGO_HOME: /home/runner/.cargo
    CARGO_INCREMENTAL: 0
    CARGO_TERM_COLOR: always
Downloading cpython-3.13.9+freethreaded-linux-x86_64-gnu (download) (110.5MiB)
 Downloading cpython-3.13.9+freethreaded-linux-x86_64-gnu (download)
Using CPython 3.13.9
Creating virtual environment at: .venv
Resolved 59 packages in 130ms
Downloading maturin (8.2MiB)
Downloading pygments (1.2MiB)
Downloading pillow (4.2MiB)
 Downloading maturin
 Downloading pillow
 Downloading pygments
Prepared 10 packages in 176ms
Installed 10 packages in 21ms
 + iniconfig==2.3.0
 + maturin==1.9.6
 + packaging==24.2
 + patchelf==0.17.2.4
 + pillow==11.0.0
 + pluggy==1.6.0
 + py-cpuinfo==9.0.0
 + pygments==2.19.1
 + pytest==8.4.0
 + pytest-benchmark==5.0.0
0s
Run VENV_PYTHON=$(uv python find --directory .venv)
  VENV_PYTHON=$(uv python find --directory .venv)
  echo "VENV_PYTHON: $VENV_PYTHON"
  $VENV_PYTHON -m maturin develop --uv
  shell: /usr/bin/bash -e {0}
  env:
    UV_PYTHON: 3.13t
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
    CARGO_HOME: /home/runner/.cargo
    CARGO_INCREMENTAL: 0
    CARGO_TERM_COLOR: always
VENV_PYTHON: /usr/bin/python3
/usr/bin/python3: No module named maturin
Error: Process completed with exit code 1.
```

</details>

The workflow file is available on the job logs [here](https://github.com/lmmx/comrak/actions/runs/18766098367/workflow?pr=28) and also attached here:

<details>
<summary>🗃️ Click to show CI workflow</summary>

```yaml
name: Test

on:
  push:
    branches:
      - master
  pull_request:
  workflow_dispatch:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref_name }}-${{ github.event.pull_request.number || github.sha }}
  cancel-in-progress: true

jobs:
  get-python-versions:
    runs-on: ubuntu-latest
    timeout-minutes: 5
    outputs:
      python-versions: ${{ steps.get-versions.outputs.python-versions }}
    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v6

      - name: Get Python versions from uv
        id: get-versions
        run: |
          MIN_VERSION=$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.requires-python | sed 's/>=//;s/"//g')
          versions=$(uv python list --all-versions --output-format json | jq -c --arg min "$MIN_VERSION" '
            ($min | split(".") | {major: .[0]|tonumber, minor: .[1]|tonumber}) as $minv |
            [.[] | {v:.version_parts, t:.variant}]
            | unique_by([.v.minor,.t])
            | map(select(.v.major > $minv.major or (.v.major == $minv.major and .v.minor >= $minv.minor)))
            | map("\(.v.major).\(.v.minor)\(if .t=="freethreaded" then "t" else "" end)")')
          echo "python-versions=$versions" >> $GITHUB_OUTPUT
  test:
    needs: get-python-versions
    runs-on: ubuntu-latest
    timeout-minutes: 10
    strategy:
      fail-fast: false
      matrix:
        python-version: ${{ fromJson(needs.get-python-versions.outputs.python-versions) }}
    steps:
      - uses: actions/checkout@v4

      # - name: Set up mold linker
      #   uses: rui314/setup-mold@v1

      # - uses: Swatinem/rust-cache@v2.8.1

      - name: Install uv
        uses: astral-sh/setup-uv@v6
        with:
          enable-cache: false # true
          python-version: ${{ matrix.python-version }}

      - name: Set up Rust
        uses: dtolnay/rust-toolchain@stable

      - name: Sync build and test dependencies
        run: uv sync --only-group build --only-group test

      - name: Build extension
        run: |
          VENV_PYTHON=$(uv python find --directory .venv)
          echo "VENV_PYTHON: $VENV_PYTHON"
          $VENV_PYTHON -m maturin develop --uv
      - name: Run tests
        run: $(uv python find) -m pytest tests/ -v
```

</details>

### Platform

ubuntu-latest

### Version

0.9.5

### Python version

3.13t (also on: 3.14t)

---

_Label `bug` added by @lmmx on 2025-10-24 01:25_

---

_Comment by @zanieb on 2025-10-24 03:45_

Can you share verbose logs for `uv python find`? e.g., `uv python find -vv`?

Separately, if your goal is to invoke Python in a virtual environment, I'd use `uv run python` instead. You can also use `uv run -m` directly.

---

_Comment by @lmmx on 2025-10-24 08:22_

> Can you share verbose logs for `uv python find`? e.g., `uv python find -vv`?

So this now **only** fails with the free-threaded pythons

- I tried removing source .venv/bin/activate and it has the same result
- I now see 3.14 (not `t`) working normally, so I think that must have been a separate failure, not related.

<details>
<summary> 📸 Click to show screenshot</summary>

![Image](https://github.com/user-attachments/assets/c2ae91f7-2533-459f-bd51-7b34f7a74311)

</details>

<details>
<summary>📝 Click to show verbose output</summary>

```
Run source .venv/bin/activate
  source .venv/bin/activate
  echo "-- uv python find VERBOSE DEBUG"
  uv python find -vv
  echo "-- END OF uv python find DEBUG"
  $(uv python find) -m maturin develop --uv
  shell: /usr/bin/bash -e {0}
  env:
    CACHE_ON_FAILURE: false
    CARGO_INCREMENTAL: 0
    UV_PYTHON_INSTALL_DIR: /home/runner/work/_temp/uv-python-dir
    UV_PYTHON: 3.13t
    CARGO_HOME: /home/runner/.cargo
    CARGO_TERM_COLOR: always
-- uv python find VERBOSE DEBUG
DEBUG uv 0.9.5
TRACE Checking shared lock for `/home/runner/.cache/uv` at `/home/runner/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/runner/.cache/uv`
DEBUG Found project root: `/home/runner/work/comrak/comrak`
DEBUG No workspace root found, using project root
DEBUG No Python version file found in workspace: /home/runner/work/comrak/comrak
DEBUG Using Python request `>=3.9` from `requires-python` metadata
DEBUG Searching for Python >=3.9 in virtual environments, managed installations, or search path
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python3
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3` (active virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from active virtual environment: does not satisfy request `>=3.9`
TRACE Found cached interpreter info for Python 3.13.9, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3` (virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `>=3.9`
DEBUG Searching for managed installations at `/home/runner/work/_temp/uv-python-dir`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `../lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ld.so --version`: "ld.so (Ubuntu GLIBC 2.39-0ubuntu8.6) stable release version 2.39.\nCopyright (C) 2024 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.39 in stdout of ld.so version
DEBUG Found managed installation `cpython-3.13.9+freethreaded-linux-x86_64-gnu`
TRACE Found cached interpreter info for Python 3.13.9, skipping query of: /home/runner/work/_temp/uv-python-dir/cpython-3.13.9+freethreaded-linux-x86_64-gnu/bin/python3.13t
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/_temp/uv-python-dir/cpython-3.13.9+freethreaded-linux-x86_64-gnu/bin/python3.13t` (managed installations)
DEBUG Skipping interpreter at `/home/runner/work/_temp/uv-python-dir/cpython-3.13.9+freethreaded-linux-x86_64-gnu/bin/python3.13t` from managed installations: does not satisfy request `>=3.9`
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /home/runner/work/comrak/comrak/.venv/bin
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3
TRACE Found cached interpreter info for Python 3.13.9, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `.venv/bin/python3` from first executable in the search path: does not satisfy request `>=3.9`
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python` (search path)
DEBUG Skipping interpreter at `.venv/bin/python` from search path: does not satisfy request `>=3.9`
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3.13
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python3.13
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3.13` (search path)
DEBUG Skipping interpreter at `.venv/bin/python3.13` from search path: does not satisfy request `>=3.9`
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3.13t
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python3.13t
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3.13t` (search path)
DEBUG Skipping interpreter at `.venv/bin/python3.13t` from search path: does not satisfy request `>=3.9`
TRACE Checking `PATH` directory for interpreters: /home/runner/work/_temp/uv-python-dir
TRACE Checking `PATH` directory for interpreters: /opt/hostedtoolcache/uv/0.9.5/x86_64
TRACE Checking `PATH` directory for interpreters: /opt/pipx_bin
TRACE Checking `PATH` directory for interpreters: /home/runner/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/.ghcup/bin
TRACE Checking `PATH` directory for interpreters: /home/runner/.dotnet/tools
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Found cached interpreter info for Python 3.12.3, skipping query of: /usr/bin/python3
/usr/bin/python3
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Released lock at `/home/runner/.cache/uv/.lock`
-- END OF uv python find DEBUG
/usr/bin/python3: No module named maturin
```

</details>

It seems to reject it!

- First as python3

```sh
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3
TRACE Found cached interpreter info for Python 3.13.9, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `.venv/bin/python3` from first executable in the search path: does not satisfy request `>=3.9`
```

- Then as python

```sh
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python` (search path)
DEBUG Skipping interpreter at `.venv/bin/python` from search path: does not satisfy request `>=3.9`
```

- Then as python13

```sh
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3.13
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python3.13
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3.13` (search path)
DEBUG Skipping interpreter at `.venv/bin/python3.13` from search path: does not satisfy request `>=3.9`
```

- Then as python13t

```sh
TRACE Found possible Python executable: /home/runner/work/comrak/comrak/.venv/bin/python3.13t
TRACE Querying interpreter executable at /home/runner/work/comrak/comrak/.venv/bin/python3.13t
DEBUG Found `cpython-3.13.9+freethreaded-linux-x86_64-gnu` at `/home/runner/work/comrak/comrak/.venv/bin/python3.13t` (search path)
DEBUG Skipping interpreter at `.venv/bin/python3.13t` from search path: does not satisfy request `>=3.9`
```

> Separately, if your goal is to invoke Python in a virtual environment, I'd use `uv run python` instead. You can also use `uv run -m` directly.

I thought that was the one that pulls the dependency at call time not uses the venv one?

I want to ensure I am using the exactly right version in CI (in this case a free-threaded Python maturin) not just let it pull in a suitable one (and mislead me that it is free-threaded if actually there is a bug in that)

---

_Renamed from "`uv python find` does not detect venv Python in CPython 3.13t and 3.14+ (CI)" to "`uv python find` does not detect venv Python in CPython 3.13t and 3.14t (CI)" by @lmmx on 2025-10-24 08:26_

---

_Comment by @lmmx on 2025-10-24 09:02_

Closing this in favour of more precise repro in #16434 

Update: 3.14t was actually failing at a later CI step due to segfault and 3.13t was failing at this uv python find step as the discovery is hardcoded to treat 3.13t as experimental and discount it during comparison to requires-python (which I think is a bug but a separate one)

The 3.14t was an unrelated SegFault, I should not have assumed. Updating issue title here apologies for misreport

```
Run source .venv/bin/activate
============================= test session starts ==============================
platform linux -- Python 3.14.0, pytest-8.4.0, pluggy-1.6.0 -- /home/runner/work/comrak/comrak/.venv/bin/python
cachedir: .pytest_cache
rootdir: /home/runner/work/comrak/comrak
configfile: pyproject.toml
Fatal Python error: Segmentation fault
```

---

_Closed by @lmmx on 2025-10-24 09:02_

---

_Renamed from "`uv python find` does not detect venv Python in CPython 3.13t and 3.14t (CI)" to "`uv python find` does not detect venv Python in CPython 3.13t" by @lmmx on 2025-10-24 09:32_

---
