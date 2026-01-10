---
number: 13490
title: "Presence of `UV_CONFIG_FILE` implicitly adds default index"
type: issue
state: closed
author: eegli
labels:
  - bug
assignees: []
created_at: 2025-05-16T13:55:43Z
updated_at: 2025-06-20T09:10:03Z
url: https://github.com/astral-sh/uv/issues/13490
synced_at: 2026-01-10T01:25:34Z
---

# Presence of `UV_CONFIG_FILE` implicitly adds default index

---

_Issue opened by @eegli on 2025-05-16 13:55_

### Update
The issue has been pinpointed; [see this comment](https://github.com/astral-sh/uv/issues/13490#issuecomment-2889718521).

### Summary

We are in a corporate scenario where we want to **always** use our private index and "disable" fetching from PyPi. According to [the docs](https://docs.astral.sh/uv/configuration/indexes/#defining-an-index), this can be done via the `default = true` entry.

Our only index specified in pyproject.toml:
```toml
[[tool.uv.index]]
name = "company-virtual"
url = "https://europe-west6-python.pkg.dev/xxx/simple"
default = true # note this!
```

To demonstrate the issue, let's assume we only have a single dependency **that is not available on our index**:
```toml
[dependency-groups]
dev = [
    "ruff>=0.11.2",
]
```
Running `uv sync --refresh --reinstall --no-cache` will actually fetch `ruff` from PyPi, see the lockfile:

```toml
[[package]]
name = "ruff"
version = "0.11.10"
source = { registry = "https://pypi.org/simple" }
```

However, specifying the default index via the cli fails **as expected**:

```sh
just uv sync --refresh --default-index company-virtual
> uv sync --refresh --default-index company-virtual
  × No solution found when resolving dependencies for split (python_full_version >= '3.11'):
  ╰─▶ Because ruff was not found in the package registry ...
```

Let me know if you need more info! Thanks

### Platform

Windows 11 (MINGW64_NT-10.0-22631 3.3.6-bec3d608-341.x86_64 x86_64 Msys)

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

3.11

---

_Label `bug` added by @eegli on 2025-05-16 13:55_

---

_Comment by @charliermarsh on 2025-05-16 13:59_

Are you able to create a minimal reproduction for us? You can use https://test.pypi.org/ if you need a non-PyPI index to use for the example.

---

_Label `bug` removed by @charliermarsh on 2025-05-16 13:59_

---

_Label `needs-mre` added by @charliermarsh on 2025-05-16 13:59_

---

_Comment by @eegli on 2025-05-16 15:11_

I tried to setup a minimal example from `uv init` and it appears that the default index is correctly registered. Sorry for the fuzz.

The actual problem may lie somewhere else and I managed to track down one weird thing. In reality, we have two indexes (provided by Google Artifact Registry):
1. one that is a PyPi mirror (second entry with `default = true`)
2. another one for internal packages (first entry)

```toml
[[tool.uv.index]]
# Internal company packages
name = "company-virtual"
url = "https://europe-west6-python.pkg.dev/xxx-virtual-repo/simple"

[[tool.uv.index]]
# Upstream repo: PyPI
name = "company-remote"
url = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple"
default = true
```

We want/need to use the `company-remote` as a default index and `company-virtual` for the internal packages as an extra index.

## Setup and commands
Given **no lockfile** and the two indexes from above for all commands:

1. This does not work
 ```sh
uv sync --all-packages --refresh --no-cache
```
> × No solution found when resolving dependencies:
  ╰─▶ Because \<internal-package\> was not found in the package registry...  we can conclude that your workspace's requirements are unsatisfiable.

2. With the **exact same pyproject file**, specifying the **default index and additional index from the command line**, it works!
```sh
uv sync --all-packages --refresh --no-cache \
    --default-index company-remote=https://europe-west6-python.pkg.dev/xxx-remote-repo/simple \
    --index company-virtual=https://europe-west6-python.pkg.dev/xxx-virtual-repo/simple
```
3. When doing step 2 with env variables instead (UV_DEFAULT_INDEX and UV_INDEX), syncing works, too!

To give some context:
- We're using workspaces with around 5 members
- Credentials are correct - otherwise, the second command above would not work
- Credentials are injected via env variables from a justfile

Let me know if I can provide anything else. And thank you for the amazing work!

## Additional info

Edit - The following info might be helpful:

**In the second (and third) scenario** where everything works, the lockfile correctly pins the index, even for non-internal packages (this is what we want):
```toml
[[package]]
name = "aiohttp"
version = "3.11.18"
source = { registry = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple" }
```

**In the first scenario**, when running with `--verbose`, some packages seem to be requested from PyPi?
```sh
DEBUG No cache entry for: https://pypi.org/simple/cffi/
DEBUG No cache entry for: https://pypi.org/simple/pywin32/
DEBUG No cache entry for: https://pypi.org/simple/python-dateutil/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/platformdirs/
DEBUG No cache entry for: https://pypi.org/simple/pycodestyle/
```

---

_Comment by @eegli on 2025-05-18 13:57_

Here is a more stripped down configuration that replicates the problem, which exists for both adding and syncing deps. For now, let's ignore the index with the private packages.

**pyproject.toml**
```toml
[project]
name = "company-name"
author = "authors"
version = "0.0.0"
readme = "README.md"
requires-python = ">=3.10, <3.11"
dependencies = []

[[tool.uv.index]]
# Upstream repo: PyPI
name = "company-remote"
url = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple"
default = true
```

**uv.lock** - does not exist

**Expectation**: uv picks up the _custom_ default index from pyproject and ignores PyPi. As per the docs:

> By default, uv includes the Python Package Index (PyPI) as the "default" index, i.e., the index used when a package is not found on any other index. To exclude PyPI from the list of indexes, set default = true on another index entry (or use the --default-index command-line option):

**Actual**: Only when the default index is specified via CLI flags or env variables, resulting in each package being explicitly associated with an index, the custom default index is actually picked up. If a `default = true` index is specified in pyproject.toml, uv still falls back to PyPi. 

### `uv add` (pyproject index)
- `uv add polars --no-cache --refresh`
👉🏾 **uv uses the PyPi index, even with a custom default index defined** ❌
```toml
# results in uv.lock
[[package]]
name = "polars"
version = "1.29.0"
source = { registry = "https://pypi.org/simple" }
```

### `uv add` (CLI/env index)
- `uv add polars --no-cache --refresh --index company-remote=https://europe-west6-python.pkg.dev/xxx-remote-repo/simple`
- `UV_DEFAULT_INDEX=company-remote=https://europe-west6-python.pkg.dev/xxx-remote-repo/simple uv add polars --no-cache --refresh`
👉🏾 **uv uses the correct index in both scenarios** ✅
```toml
# results in uv.lock
[[package]]
name = "polars"
version = "1.29.0"
source = { registry = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple" }
```
This will add this entry to pyproject.toml:
```toml
[tool.uv.sources]
polars = { index = "company-remote" }
```
After which the first command, of course, works too.

### `uv sync` (pyproject index)
The problem is similar when syncing an environment. Assume we have the same pyproject as before but now with polars already specified:
```toml
dependencies = [
    "polars>=1.29.0",
]
```
- `just uv sync --no-cache --refresh`
👉🏾The resolution again falls back to the PyPi
```toml
# results in uv.lock
[[package]]
name = "polars"
version = "1.29.0"
source = { registry = "https://pypi.org/simple" }
```

---

_Comment by @charliermarsh on 2025-05-18 14:38_

Unfortunately, I can't reproduce that. I ran `uv init foo`, then edited the `pyproject.toml` to:

```toml
[project]
name = "company-name"
author = "authors"
version = "0.0.0"
readme = "README.md"
requires-python = ">=3.10, <3.11"
dependencies = []

[[tool.uv.index]]
# Upstream repo: PyPI
name = "company-remote"
url = "https://test.pypi.org/simple"
default = true
```

Then ran `uv add iniconfig`, and the `uv.lock` contains:

```toml
version = 1
revision = 2
requires-python = "==3.10.*"

[[package]]
name = "company-name"
version = "0.0.0"
source = { virtual = "." }
dependencies = [
    { name = "iniconfig" },
]

[package.metadata]
requires-dist = [{ name = "iniconfig", specifier = ">=2.0.0" }]

[[package]]
name = "iniconfig"
version = "2.0.0"
source = { registry = "https://test.pypi.org/simple" }
sdist = { url = "https://test-files.pythonhosted.org/packages/d7/4b/cbd8e699e64a6f16ca3a8220661b5f83792b3017d0f79807cb8708d33913/iniconfig-2.0.0.tar.gz", hash = "sha256:2d91e135bf72d31a410b17c16da610a82cb55f6b0477d1a902134b24a455b8b3", size = 4646, upload-time = "2023-01-07T11:08:16.826Z" }
wheels = [
    { url = "https://test-files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl", hash = "sha256:b6a85871a79d2e3b22d2d1b94ac2824226a63c6b741c88f7ae975f18b6778374", size = 5892, upload-time = "2023-01-07T11:08:14.843Z" },
]
```

---

_Comment by @eegli on 2025-05-18 15:40_

I also cannot reproduce the issue outside of our corporate setup using test.pypi.org. Let's close the issue, could be that there are other factors involved outside of uv's control, such as our internal HTTP proxy. For now, we'll be using env variables for explicit resolution - it all works with those in our case.

Thanks a lot for looking into it! 

---

_Closed by @eegli on 2025-05-18 15:40_

---

_Comment by @charliermarsh on 2025-05-18 17:21_

I don't _think_ I'd expect it to manifest this way, but some indexes will automatically redirect to PyPI if you provide invalid credentials or similar. That _could_ be what's going on here?

---

_Comment by @eegli on 2025-05-19 05:29_

I have traced that and could not find a redirect. Looking at the requests through my local HTTP proxy from the _previous_ example, all requests go directly to pypi.org. Using `--verbose` when the index is **only** defined in pyproject gives me the entry:
> DEBUG Found static `pyproject.toml` for: temp @ file:///C:/Users/me-x/Localdata/Git/temp
> DEBUG No cache entry for: https://pypi.org/simple/polars/

However, I noticed that if I _move_ the index from pyproject to `uv.toml`, **it is picked up correctly**:

```toml
# pyproject
[project]
name = "temp"
version = "0.0.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = ["polars>=1.29.0"]
```

```toml
# uv.toml
[[index]]
# Upstream repo: PyPI
name = "company-remote"
url = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple"
default = true
```

Runinng `just uv sync --refresh --no-cache --verbose` **works** and results in the lockfile:
```toml
[[package]]
name = "polars"
version = "1.29.0"
source = { registry = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple" }
```
> DEBUG Found static `pyproject.toml` for: temp @ file:///C:/Users/me-x/Localdata/Git/temp
> DEBUG No cache entry for: https://europe-west6-python.pkg.dev/xxx-remote-repo/simple/polars/


It almost appears as if specifying the index in pyproject is the issue on my Windows machine. Do you have any idea on how to debug this? 


---

_Reopened by @eegli on 2025-05-19 05:29_

---

_Comment by @eegli on 2025-05-19 06:01_

The issue has now been identified. In all previous examples, I've had a minimal `uv.toml` config file for my user:
```toml
native-tls = true
```
It appears that the **sheer presence of a user** `uv.toml` config - **even if empty** - results in the PyPi being included as the default index.

Getting rid of the config file (`unset UV_CONFIG_FILE`) and relying solely on the following pyproject config:
```toml
[project]
name = "temp"
version = "0.0.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = ["polars>=1.29.0"]

[[tool.uv.index]]
# Upstream repo: PyPI
name = "company-remote"
url = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple"
default = true
```
Runinng `uv sync --refresh --no-cache --verbose` **works** and results in the lockfile:

```toml
[[package]]
name = "polars"
version = "1.29.0"
source = { registry = "https://europe-west6-python.pkg.dev/xxx-remote-repo/simple" }
```

Demo repo to reproduce: [eegli/uv-issue-13490](https://github.com/eegli/uv-issue-13490).

### Details

While the default configuration file resolution order works for most entries (they default to some nullish value), it is problematic for `ResolverInstallerOptions.index` because it defines PyPi as a default:
https://github.com/astral-sh/uv/blob/6afb11ccf639523b3cdba085d3315815ccacd182/crates/uv-settings/src/settings.rs#L411
Upon merging the configuration in
https://github.com/astral-sh/uv/blob/6afb11ccf639523b3cdba085d3315815ccacd182/crates/uv/src/lib.rs#L122
PyPi is therefore always included when any `uv.toml` is present. My expectation is that this implicit default is not included if another default index is specified explicitly. 

As much as I'd love to provide a fix, I don't have the capacity right now to familiarize myself with the source.

---

_Renamed from "uv does not respect pyproject.toml default index" to "presence of `uv.toml` implicitly overrides default index in `pyproject.toml`" by @eegli on 2025-05-19 06:03_

---

_Renamed from "presence of `uv.toml` implicitly overrides default index in `pyproject.toml`" to "Presence of `uv.toml` implicitly overrides default index in `pyproject.toml`" by @eegli on 2025-05-19 06:38_

---

_Renamed from "Presence of `uv.toml` implicitly overrides default index in `pyproject.toml`" to "Presence of `UV_CONFIG_FILE` implicitly adds default index" by @konstin on 2025-05-19 07:42_

---

_Label `needs-mre` removed by @konstin on 2025-05-19 07:42_

---

_Label `bug` added by @konstin on 2025-05-19 07:42_

---

_Comment by @konstin on 2025-05-19 07:44_

Specifically, the presence of `UV_CONFIG_FILE` causes this, a `~/.config/uv/uv.toml` is not enough.

---

_Comment by @eegli on 2025-05-19 07:53_

I have not yet checked the effect of user and system-level configuration. But yes, both explicit `UV_CONFIG_FILE` and its alternative `--config-file` are affected.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-05-19 09:25_

---

_Referenced in [astral-sh/uv#13752](../../astral-sh/uv/issues/13752.md) on 2025-05-31 09:07_

---

_Comment by @eegli on 2025-05-31 15:38_

It appears that this is the correct behavior. According to the docs:
> uv also accepts a --config-file command-line argument, which accepts a path to a uv.toml to use as the configuration file. When provided, this file will be used in place of _any_ discovered configuration files (e.g., user-level configuration will be ignored).

> uv.toml files take precedence over pyproject.toml files, so if both uv.toml and pyproject.toml files are present in a directory, configuration will be read from uv.toml, and [tool.uv] section in the accompanying pyproject.toml will be ignored.

I always thought that settings are not only merged when discovering system/user/project configuration but also for explicitly set `uv.toml` files. My bad!



---

_Closed by @eegli on 2025-05-31 15:38_

---

_Unassigned @jtfmumm by @jtfmumm on 2025-06-20 09:10_

---
