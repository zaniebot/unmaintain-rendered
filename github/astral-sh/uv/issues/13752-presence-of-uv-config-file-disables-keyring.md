---
number: 13752
title: Presence of UV_CONFIG_FILE disables keyring provider in pyproject.toml
type: issue
state: closed
author: eegli
labels:
  - bug
assignees: []
created_at: 2025-05-31T09:06:39Z
updated_at: 2025-05-31T15:38:59Z
url: https://github.com/astral-sh/uv/issues/13752
synced_at: 2026-01-07T13:12:18-06:00
---

# Presence of UV_CONFIG_FILE disables keyring provider in pyproject.toml

---

_Issue opened by @eegli on 2025-05-31 09:06_

### Summary

Assume this pyproject.toml with `keyring-proivider = "subprocess"`:

```toml
[project]
name = "foo"
version = "1.0.0"
requires-python = ">=3.11, <4"
dependencies = []
[tool.uv]
keyring-provider = "subprocess"
```

and an **empty** `custom-uv.toml` in the same directory.

✅ Keyring is picked up from pyproject.toml when no config file is specified:
```sh
uv sync --show-settings | grep keyring
>             keyring_provider: Subprocess,
```
❌ Specifying a config file with no keyring sets `keyring_provider` to disabled:
```sh
uv sync --config-file custom-uv.toml --show-settings | grep keyring
>            keyring_provider: Disabled,
```
likewise
```sh
UV_CONFIG_FILE=custom-uv.toml uv sync --show-settings | grep keyring
>            keyring_provider: Disabled,
```
This is related to #13490.
 

### Platform

MINGW64_NT-10.0-26100 3.5.7-463ebcdc.x86_64 x86_64 Msys

### Version

uv 0.7.8 (0ddcc1905 2025-05-23)

### Python version

3.13

---

_Label `bug` added by @eegli on 2025-05-31 09:06_

---

_Comment by @eegli on 2025-05-31 09:43_

Here's a snapshot test to be used:

```rs
#[test]
#[cfg_attr(
    windows,
    ignore = "Configuration tests are not yet supported on Windows"
)]
fn add_keyring_provider_conflict() -> anyhow::Result<()> {
    let context = TestContext::new("3.12");

    let config = context.temp_dir.child("custom-uv.toml");
    config.write_str(indoc::indoc! {r#"
    "#})?;

    let pyproject_toml = context.temp_dir.child("pyproject.toml");
    pyproject_toml.write_str(indoc::indoc! {r#"
        [project]
        name = "project"
        version = "0.1.0"
        requires-python = ">=3.12"
        [tool.uv]
        keyring-provider = "subprocess"
    "#})?;

    uv_snapshot!(context.filters(), add_shared_args(context.lock(), context.temp_dir.path())
        .arg("--show-settings")
        .arg("--config-file")
        .arg(config.path()),
        @r#"
    success: true
    exit_code: 0
    ----- stdout -----
    GlobalSettings {
        required_version: None,
        quiet: 0,
        verbose: 0,
        color: Auto,
        network_settings: NetworkSettings {
            connectivity: Online,
            native_tls: false,
            allow_insecure_host: [],
        },
        concurrency: Concurrency {
            downloads: 50,
            builds: 16,
            installs: 8,
        },
        show_settings: true,
        preview: Disabled,
        python_preference: Managed,
        python_downloads: Automatic,
        no_progress: false,
        installer_metadata: true,
    }
    CacheSettings {
        no_cache: false,
        cache_dir: Some(
            "[CACHE_DIR]/",
        ),
    }
    LockSettings {
        locked: false,
        frozen: false,
        dry_run: Disabled,
        script: None,
        python: None,
        install_mirrors: PythonInstallMirrors {
            python_install_mirror: None,
            pypy_install_mirror: None,
            python_downloads_json_url: None,
        },
        refresh: None(
            Timestamp(
                SystemTime {
                    tv_sec: [TIME],
                    tv_nsec: [TIME],
                },
            ),
        ),
        settings: ResolverSettings {
            build_options: BuildOptions {
                no_binary: None,
                no_build: None,
            },
            config_setting: ConfigSettings(
                {},
            ),
            dependency_metadata: DependencyMetadata(
                {},
            ),
            exclude_newer: None,
            fork_strategy: RequiresPython,
            index_locations: IndexLocations {
                indexes: [],
                flat_index: [],
                no_index: false,
            },
            index_strategy: FirstIndex,
            keyring_provider: Subprocess,
            link_mode: Clone,
            no_build_isolation: false,
            no_build_isolation_package: [],
            prerelease: IfNecessaryOrExplicit,
            resolution: Highest,
            sources: Enabled,
            upgrade: None,
        },
    }

    ----- stderr -----
    "#);

    Ok(())
}
```

This fails with:

```sh
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────
   65    65 │             flat_index: [],
   66    66 │             no_index: false,
   67    67 │         },
   68    68 │         index_strategy: FirstIndex,
   69       │-        keyring_provider: Subprocess,
         69 │+        keyring_provider: Disabled,
   70    70 │         link_mode: Clone,
   71    71 │         no_build_isolation: false,
   72    72 │         no_build_isolation_package: [],
   73    73 │         prerelease: IfNecessaryOrExplicit,
```

---

_Comment by @charliermarsh on 2025-05-31 14:13_

Is this not the same as https://github.com/astral-sh/uv/issues/13490? Using a config file disables reading configuration from the project.

---

_Comment by @eegli on 2025-05-31 14:27_

I see it now:
> uv also accepts a --config-file command-line argument, which accepts a path to a uv.toml to use as the configuration file. When provided, this file will be used in place of any discovered configuration files (e.g., user-level configuration will be ignored).

Sorry for the fuzz - I always assumed a (partial) `uv.toml` would be merged with `pyproject.toml` when specified via `--config-file` because user/system/project level settings are also merged.

In short, if I want to merge settings from `uv.toml` files, I have to place them _exactly_ in the default user/system config locations?

---

_Closed by @eegli on 2025-05-31 15:39_

---
