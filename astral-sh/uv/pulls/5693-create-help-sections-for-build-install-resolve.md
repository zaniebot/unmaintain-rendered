```yaml
number: 5693
title: Create help sections for build, install, resolve, and index
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/help-sections
created_at: 2024-08-01T16:36:37Z
updated_at: 2024-08-05T21:28:36Z
url: https://github.com/astral-sh/uv/pull/5693
synced_at: 2026-01-10T13:31:54Z
```

# Create help sections for build, install, resolve, and index

---

_Pull request opened by @zanieb on 2024-08-01 16:36_

Part of #4454 

e.g.

```
$ uv add --help
Add one or more packages to the project requirements

Usage: uv add [OPTIONS] <REQUIREMENTS>...

Arguments:
  <REQUIREMENTS>...  The packages to add, as PEP 508 requirements (e.g., `ruff==0.5.0`)

Options:
      --dev                  Add the requirements as development dependencies
      --optional <OPTIONAL>  Add the requirements to the specified optional dependency group
      --no-editable          Don't add the requirements as editables
      --raw-sources          Add source requirements to `project.dependencies`, rather than `tool.uv.sources`
      --rev <REV>            Specific commit to use when adding from Git
      --tag <TAG>            Tag to use when adding from git
      --branch <BRANCH>      Branch to use when adding from git
      --extra <EXTRA>        Extras to activate for the dependency; may be provided more than once
      --locked               Assert that the `uv.lock` will remain unchanged
      --frozen               Add the requirements without updating the `uv.lock` file
      --package <PACKAGE>    Add the dependency to a specific package in the workspace
  -p, --python <PYTHON>      The Python interpreter into which packages should be installed. [env: UV_PYTHON=]

Index options:
  -i, --index-url <INDEX_URL>                The URL of the Python package index (by default: <https://pypi.org/simple>) [env: UV_INDEX_URL=]
      --extra-index-url <EXTRA_INDEX_URL>    Extra URLs of package indexes to use, in addition to `--index-url` [env: UV_EXTRA_INDEX_URL=]
  -f, --find-links <FIND_LINKS>              Locations to search for candidate distributions, in addition to those found in the registry indexes
      --no-index                             Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
      --index-strategy <INDEX_STRATEGY>      The strategy to use when resolving against multiple index URLs [env: UV_INDEX_STRATEGY=] [possible values: first-index, unsafe-first-match, unsafe-best-match]
      --keyring-provider <KEYRING_PROVIDER>  Attempt to use `keyring` for authentication for index URLs [env: UV_KEYRING_PROVIDER=] [possible values: disabled, subprocess]

Resolver options:
  -U, --upgrade                            Allow package upgrades, ignoring pinned versions in any existing output file
  -P, --upgrade-package <UPGRADE_PACKAGE>  Allow upgrades for a specific package, ignoring pinned versions in any existing output file
      --resolution <RESOLUTION>            The strategy to use when selecting between the different compatible versions for a given package requirement [env: UV_RESOLUTION=] [possible values: highest, lowest, lowest-direct]
      --prerelease <PRERELEASE>            The strategy to use when considering pre-release versions [env: UV_PRERELEASE=] [possible values: disallow, allow, if-necessary, explicit, if-necessary-or-explicit]
      --exclude-newer <EXCLUDE_NEWER>      Limit candidate packages to those that were uploaded prior to the given date [env: UV_EXCLUDE_NEWER=]

Installer options:
      --reinstall                              Reinstall all packages, regardless of whether they're already installed. Implies `--refresh`
      --reinstall-package <REINSTALL_PACKAGE>  Reinstall a specific package, regardless of whether it's already installed. Implies `--refresh-package`
      --link-mode <LINK_MODE>                  The method to use when installing packages from the global cache [env: UV_LINK_MODE=] [possible values: clone, copy, hardlink, symlink]
      --compile-bytecode                       Compile Python files to bytecode after installation

Build options:
  -C, --config-setting <CONFIG_SETTING>        Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
      --no-build                               Don't build source distributions
      --no-build-package <NO_BUILD_PACKAGE>    Don't build source distributions for a specific package
      --no-binary                              Don't install pre-built wheels
      --no-binary-package <NO_BINARY_PACKAGE>  Don't install pre-built wheels for a specific package

Cache options:
  -n, --no-cache                           Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>              Path to the cache directory [env: UV_CACHE_DIR=]
      --refresh                            Refresh all cached data
      --refresh-package <REFRESH_PACKAGE>  Refresh cached data for a specific package

Python options:
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv [possible values: only-managed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic, manual]

Global options:
  -q, --quiet                      Do not print any output
  -v, --verbose...                 Use verbose output
      --color <COLOR_CHOICE>       Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls                 Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
      --offline                    Disable network access, relying only on locally cached data and locally available files
      --no-progress                Hides all progress outputs when set
      --config-file <CONFIG_FILE>  The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
      --no-config                  Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) in the current directory, parent directories, or user configuration directories [env: UV_NO_CONFIG=]
  -h, --help                       Print help
  -V, --version                    Print version

Use `uv help add` for more details.
```

---

_Label `cli` added by @zanieb on 2024-08-01 16:36_

---

_Comment by @zanieb on 2024-08-01 16:38_

cc @eth3lbert 

---

_Review requested from @charliermarsh by @zanieb on 2024-08-01 16:43_

---

_@eth3lbert approved on 2024-08-01 17:02_

LGTM 👍 
(I don't actually have a preference for how they should be categorized.)

---

_Comment by @eth3lbert on 2024-08-01 17:05_

I am curious why the CI only fails on Windows.


---

_Comment by @zanieb on 2024-08-01 17:10_

Some stack size BS 😬 

---

_Merged by @zanieb on 2024-08-05 21:28_

---

_Closed by @zanieb on 2024-08-05 21:28_

---

_Branch deleted on 2024-08-05 21:28_

---
