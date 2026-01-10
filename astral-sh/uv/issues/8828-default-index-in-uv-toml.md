```yaml
number: 8828
title: "Default index in `uv.toml`"
type: issue
state: closed
author: fchareyr
labels:
  - question
assignees: []
created_at: 2024-11-05T13:10:48Z
updated_at: 2024-11-05T15:00:13Z
url: https://github.com/astral-sh/uv/issues/8828
synced_at: 2026-01-10T04:36:20Z
```

# Default index in `uv.toml`

---

_Issue opened by @fchareyr on 2024-11-05 13:10_

Working in a corporate setup with a PyPi mirror, we used to define the `[[tool.uv.index]]` in `pyproject.toml`. However, we are now working jointly on a project with another firm that does not have access to our mirror.

It seemed logical to specify the location of our mirror in `uv.toml` at the system level and let the `pyproject.toml` free of any index definition, however it does not seem supported as one cannot set something equivalent to `default = "true"` in `uv.toml`.

Did I miss anything to define a default mirror at the system level? Or is there another way to address this specific case?

---

_Comment by @charliermarsh on 2024-11-05 13:19_

I would expect this to work in a `uv.toml`:

```toml
[[tool.uv.index]]
url = "..."
default = true
```

Are you seeing otherwise?

---

_Label `question` added by @charliermarsh on 2024-11-05 13:19_

---

_Comment by @fchareyr on 2024-11-05 13:28_

It tries to reach `pypi.org` unfortunately.

`pyproject.toml`
```toml
[project]
name = "poc"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "François Chareyron", email = "francois.chareyron@***.ch" }
]
requires-python = ">=3.12"
dependencies = [
    "fastapi[all]>=0.115.4",
    "mkdocs-material>=9.5.43",
    "prefect>=3.1.0",
    "pyinstrument>=5.0.0",
    "pytest>=8.3.3",
    "pytest-cov>=6.0.0",
    "pytest-sugar>=1.0.0",
    "pytest-xdist>=3.6.1",
    "sqlalchemy>=2.0.36",
    "streamlit>=1.39.0",
    "streamlit-extras>=0.5.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.prefect]
logging.level = "DEBUG"
```

`/etc/uv/uv.toml`
```toml
[[tool.uv.index]]
name = "internal"
url = "https://***.***.com/repository/pypi/simple"
default = true

[[tool.uv.index]]
name = "bloomberg"
url = "https://***.***.com/repository/bcms/repository/releases/python/simple"
```

---

_Comment by @charliermarsh on 2024-11-05 13:29_

Can you share the output of `uv sync --show-settings`?

---

_Comment by @fchareyr on 2024-11-05 13:30_

Sure thing. See below
```
GlobalSettings {
    quiet: false,
    verbose: 0,
    color: Auto,
    native_tls: false,
    concurrency: Concurrency {
        downloads: 50,
        builds: 8,
        installs: 8,
    },
    connectivity: Online,
    show_settings: true,
    preview: Disabled,
    python_preference: Managed,
    python_downloads: Automatic,
    no_progress: false,
}
CacheSettings {
    no_cache: false,
    cache_dir: None,
}
SyncSettings {
    locked: false,
    frozen: false,
    extras: None,
    dev: DevGroupsSpecification {
        dev: None,
        groups: None,
    },
    editable: Editable,
    install_options: InstallOptions {
        no_install_project: false,
        no_install_workspace: false,
        no_install_package: [],
    },
    modifications: Exact,
    package: None,
    python: None,
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1730813378,
                tv_nsec: 168272861,
            },
        ),
    ),
    settings: ResolverInstallerSettings {
        index_locations: IndexLocations {
            indexes: [],
            flat_index: [],
            no_index: false,
        },
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
        allow_insecure_host: [],
        resolution: Highest,
        prerelease: IfNecessaryOrExplicit,
        dependency_metadata: DependencyMetadata(
            {},
        ),
        config_setting: ConfigSettings(
            {},
        ),
        no_build_isolation: false,
        no_build_isolation_package: [],
        exclude_newer: None,
        link_mode: Hardlink,
        compile_bytecode: false,
        sources: Enabled,
        upgrade: None,
        reinstall: None,
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
    },
}
```

---

_Comment by @charliermarsh on 2024-11-05 13:31_

It looks like it's not finding your `/etc/uv/uv.toml` at all -- otherwise we'd see entries under `index_locations`. Are you on a fairly recent version? (That's a new-ish feature.)

---

_Comment by @fchareyr on 2024-11-05 13:34_

```
uv --version
uv 0.4.29
```
A permission issue to access `/etc/uv/uv.toml` maybe?

---

_Comment by @charliermarsh on 2024-11-05 13:57_

Does `--verbose` show anything useful?

---

_Comment by @fchareyr on 2024-11-05 14:04_

Not really - just indicating it's trying to hit `pypi.org`
```
DEBUG uv 0.4.30
DEBUG Found project root: `/home/fchareyron/***/poc`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/home/fchareyron/***/poc/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: poc @ file:///home/fchareyron/***/poc
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: poc*
DEBUG Searching for a compatible version of poc @ file:///home/fchareyron/***/poc (*)
DEBUG Adding transitive dependency for poc==0.1.0: fastapi>=0.115.4
DEBUG Adding transitive dependency for poc==0.1.0: fastapi[all]>=0.115.4
DEBUG Adding transitive dependency for poc==0.1.0: mkdocs-material>=9.5.43
DEBUG Adding transitive dependency for poc==0.1.0: prefect>=3.1.0
DEBUG Adding transitive dependency for poc==0.1.0: pyinstrument>=5.0.0
DEBUG Adding transitive dependency for poc==0.1.0: pytest>=8.3.3
DEBUG Adding transitive dependency for poc==0.1.0: pytest-cov>=6.0.0
DEBUG Adding transitive dependency for poc==0.1.0: pytest-sugar>=1.0.0
DEBUG Adding transitive dependency for poc==0.1.0: pytest-xdist>=3.6.1
DEBUG Adding transitive dependency for poc==0.1.0: sqlalchemy>=2.0.36
DEBUG Adding transitive dependency for poc==0.1.0: streamlit>=1.39.0
DEBUG Adding transitive dependency for poc==0.1.0: streamlit-extras>=0.5.0
DEBUG No cache entry for: https://pypi.org/simple/fastapi/
DEBUG No cache entry for: https://pypi.org/simple/mkdocs-material/
DEBUG No cache entry for: https://pypi.org/simple/pyinstrument/
DEBUG No cache entry for: https://pypi.org/simple/prefect/
DEBUG No cache entry for: https://pypi.org/simple/pytest-cov/
DEBUG No cache entry for: https://pypi.org/simple/pytest/
DEBUG No cache entry for: https://pypi.org/simple/pytest-sugar/
DEBUG No cache entry for: https://pypi.org/simple/pytest-xdist/
DEBUG No cache entry for: https://pypi.org/simple/sqlalchemy/
DEBUG No cache entry for: https://pypi.org/simple/streamlit/
DEBUG No cache entry for: https://pypi.org/simple/streamlit-extras/
```

---

_Comment by @charliermarsh on 2024-11-05 14:06_

We should be failing hard if we fail to read the file. Although we do check `candidate.is_file()`, which could be returning `false` if we don't have permission?

---

_Comment by @charliermarsh on 2024-11-05 14:26_

I can add a debug warning for that failure case. But what _are_ the permissions on `/etc/uv/uv.toml`?

---

_Comment by @fchareyr on 2024-11-05 14:44_

Indeed, a warning in case of permission would be useful. I've modified the permission to align with other files in `/etc` and I now have the following error:

```
❯ uv sync -vvv
error: Failed to parse: `/etc/uv/uv.toml`
  Caused by: TOML parse error at line 6, column 1
  |
6 | [index]
  | ^
invalid table header
duplicate key `index` in document root
```

As a reminder, the `uv.toml` looks like this:
```toml
[index]
name = "internal"
url = "https://***.***.com/repository/pypi/simple"
default = true

[index]
name = "bloomberg"
url = "https://***.***.com/repository/bcms/repository/releases/python/simple"
```

The error makes sense but not sure how I can consolidate into a single `index` table whilst providing the `default = true` key.

---

_Comment by @charliermarsh on 2024-11-05 14:55_

I think you need to change `[index]` to `[[index]]`.

---

_Comment by @fchareyr on 2024-11-05 14:58_

That works perfectly now, thanks very very much for your help!

---

_Closed by @fchareyr on 2024-11-05 14:58_

---

_Comment by @charliermarsh on 2024-11-05 15:00_

No problem! (That's a TOML thing, it's short-hand for like... `index = [{ name = "internal", ... }, { name = "bloomberg", ... }]`.)

---
