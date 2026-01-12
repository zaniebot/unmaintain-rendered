```yaml
number: 5022
title: "\"uv --preview lock\" does not read uv.toml or pyproject.toml settings"
type: issue
state: closed
author: dsully
labels:
  - error messages
assignees: []
created_at: 2024-07-12T19:44:35Z
updated_at: 2024-07-12T21:50:05Z
url: https://github.com/astral-sh/uv/issues/5022
synced_at: 2026-01-12T15:58:53Z
```

# "uv --preview lock" does not read uv.toml or pyproject.toml settings

---

_@dsully_

The `uv --preview lock` command does not find/respect settings in either a `pyproject.toml` or a `uv.toml` file, and thus resolution fails for internal Pypi repositories:

```toml
[pip]
extra-index-url = [
  "https://artifactory/artifactory/api/pypi/pypi-internal/simple",
  "https://artifactory/artifactory/api/pypi/pypi-external/simple",
]
index-url = "https://server/pypi/simple"
```

Or in `pyproject.toml`:
```toml
[tool.uv.pip]
extra-index-url = [
  "https://artifactory/artifactory/api/pypi/pypi-internal/simple",
  "https://artifactory/artifactory/api/pypi/pypi-external/simple",
]
index-url = "https://server/pypi/simple"
```

```shell
$ uv --preview lock --show-settings
...
LockSettings {
    python: None,
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1720813419,
                tv_nsec: 61301000,
            },
        ),
    ),
    settings: ResolverSettings {
        index_locations: IndexLocations {
            index: None,
            extra_index: [],
            flat_index: [],
            no_index: false,
        },
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
        resolution: Highest,
        prerelease: IfNecessaryOrExplicit,
        config_setting: ConfigSettings(
            {},
        ),
        exclude_newer: None,
        link_mode: Clone,
        upgrade: None,
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
    },
}
```

Passing `--index-url` and `--extra-index-url` on the command line *does* work.

---

_Comment by @zanieb on 2024-07-12 19:47_

Hi! We don't read the `tool.uv.pip` settings outside the `uv pip` CLI — instead use the top-level `tool.uv` options that correspond to their pip-specific counterparts.

---

_Label `question` added by @zanieb on 2024-07-12 19:47_

---

_Comment by @dsully on 2024-07-12 20:00_

That doesn't appear to work with `pyproject.toml`:

```toml
[tool.uv]     ■ Additional properties are not allowed ('extra-index-url', 'index-url' were unexpected)
extra-index-url =
```

And emits a schema validation error. 

In `uv.toml` if I remove any section wrapper and just have the raw key/value assignments, that works, but emits the same schema validation error.

Will `uv pip` inherit these settings? Otherwise there will need to be duplication.

---

_Comment by @zanieb on 2024-07-12 20:36_

Both of the following work for me?

```toml
[tool.uv]
index-url = "https://test.pypi.org/simple"
extra-index-url = ["https://test.pypi.org/simple"]
```

Are you on the latest version?

Yeah `uv pip` will inherit these settings. If you set them in `[tool.uv.pip]` that will take precedence over the top-level one.

---

_Comment by @dsully on 2024-07-12 21:00_

Yes - ```uv 0.2.24 (527b711bc 2024-07-10)```

I think I found the issue. If there is an empty or commented out `uv.toml` then the values in `pyproject.toml` won't be read.

There's also the need to update the schema JSON file as well.

---

_Comment by @charliermarsh on 2024-07-12 21:18_

Do you mean, the JSON Schema on SchemaStore? Definitely need to update that.

We should probably warn if you have both a `uv.toml` and a `pyproject.toml` with a `[tool.uv]` table in the same directory.

---

_Label `question` removed by @charliermarsh on 2024-07-12 21:18_

---

_Label `error messages` added by @charliermarsh on 2024-07-12 21:18_

---

_Comment by @dsully on 2024-07-12 21:23_

Yes - the JSON Schema on SchemaStore.

---

_Comment by @charliermarsh on 2024-07-12 21:24_

https://github.com/SchemaStore/schemastore/pull/3919

---

_Comment by @charliermarsh on 2024-07-12 21:25_

I'll use this issue to track adding a warning.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 21:42_

---

_Closed by @charliermarsh on 2024-07-12 21:50_

---

_Closed by @charliermarsh on 2024-07-12 21:50_

---
