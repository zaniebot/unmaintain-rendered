---
number: 9308
title: "`uv build` does not respect extra-index-url configured in `uv.toml`"
type: issue
state: closed
author: befelix
labels:
  - needs-mre
assignees: []
created_at: 2024-11-21T10:45:03Z
updated_at: 2024-11-22T07:12:55Z
url: https://github.com/astral-sh/uv/issues/9308
synced_at: 2026-01-10T01:24:39Z
---

# `uv build` does not respect extra-index-url configured in `uv.toml`

---

_Issue opened by @befelix on 2024-11-21 10:45_

In our `pyproject.toml` file, our `build-system` dependencies list a package that is only available on a private index. Running `uv build` complains that this package cannot be found, even though the index is configured in `~/.config/uv/uv.toml`. Both `uv pip install` and `python -m build` work fine. Also manually passing the relevant `--extra-index-url` flag to `uv build` resolves the issue.

Would it be possible for `uv build` to respect the index configured in the `uv.toml` file?

- platform: Linux
- uv version: `0.5.4`


---

_Comment by @charliermarsh on 2024-11-21 13:54_

As far as I can tell it is working on my end. What does `uv build --show-settings` give you? Is the index not present? How are you determining that it isn't being picked up?

---

_Closed by @charliermarsh on 2024-11-21 13:54_

---

_Reopened by @charliermarsh on 2024-11-21 13:54_

---

_Comment by @charliermarsh on 2024-11-21 13:54_

(Sorry, didn't mean to close.)

---

_Label `needs-mre` added by @charliermarsh on 2024-11-21 13:55_

---

_Comment by @befelix on 2024-11-21 14:37_

Thanks for the fast response:
```
$ uv build --show-settings

GlobalSettings {
    quiet: false,
    verbose: 0,
    color: Auto,
    native_tls: false,
    concurrency: Concurrency {
        downloads: 50,
        builds: 12,
        installs: 12,
    },
    connectivity: Online,
    allow_insecure_host: [],
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
BuildSettings {
    src: None,
    package: None,
    all_packages: false,
    out_dir: None,
    sdist: false,
    wheel: false,
    build_logs: true,
    build_constraints: [],
    hash_checking: Some(
        Verify,
    ),
    python: None,
    install_mirrors: PythonInstallMirrors {
        python_install_mirror: None,
        pypy_install_mirror: None,
    },
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1732199819,
                tv_nsec: 227167358,
            },
        ),
    ),
    settings: ResolverSettings {
        index_locations: IndexLocations {
            indexes: [],
            flat_index: [],
            no_index: false,
        },
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
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
        upgrade: None,
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
        sources: Enabled,
    },
}
```

---

_Comment by @charliermarsh on 2024-11-21 14:39_

Are other settings (apart from index) respected? And / or when running other commands, like `uv pip compile - --show-settings`?

---

_Comment by @befelix on 2024-11-21 14:50_

We don't have any other config settings except for the index in the `uv.toml` file. `uv pip compile` works as expected. Also the corresponding settings show the index:

```
$ uv pip compile - --show-settings

GlobalSettings {
    quiet: false,
    verbose: 0,
    color: Auto,
    native_tls: false,
    concurrency: Concurrency {
        downloads: 50,
        builds: 12,
        installs: 12,
    },
    connectivity: Online,
    allow_insecure_host: [],
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
PipCompileSettings {
    src_file: [
        "-",
    ],
    constraints: [],
    overrides: [],
    build_constraints: [],
    constraints_from_workspace: [],
    overrides_from_workspace: [],
    environments: SupportedEnvironments(
        [],
    ),
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1732200497,
                tv_nsec: 940355939,
            },
        ),
    ),
    settings: PipSettings {
        index_locations: IndexLocations {
            indexes: [
                Index {
                    name: None,
                    url: Url(
                        VerbatimUrl {
                            url: Url {
                                scheme: "https",
                                cannot_be_a_base: false,
                                username: "XXX",
                                password: Some(
                                    "XXX",
                                ),
                                host: Some(
                                    Domain(
                                        "XXX",
                                    ),
                                ),
                                port: None,
                                path: "XXX",
                                query: None,
                                fragment: None,
                            },
                            given: Some(
                                "XXX",
                            ),
                        },
                    ),
                    explicit: false,
                    default: false,
                    origin: None,
                },
                Index {
                    name: None,
                    url: Url(
                        VerbatimUrl {
                            url: Url {
                                scheme: "https",
                                cannot_be_a_base: false,
                                username: "XXX",
                                password: Some(
                                    "XXX",
                                ),
                                host: Some(
                                    Domain(
                                        "XXX",
                                    ),
                                ),
                                port: None,
                                path: "XXX",
                                query: None,
                                fragment: None,
                            },
                            given: Some(
                                "XXX",
                            ),
                        },
                    ),
                    explicit: false,
                    default: false,
                    origin: None,
                },
                Index {
                    name: None,
                    url: Url(
                        VerbatimUrl {
                            url: Url {
                                scheme: "https",
                                cannot_be_a_base: false,
                                username: "XXX",
                                password: Some(
                                    "XXX",
                                ),
                                host: Some(
                                    Domain(
                                        "XXX",
                                    ),
                                ),
                                port: None,
                                path: "XXX",
                                query: None,
                                fragment: None,
                            },
                            given: Some(
                                "XXX",
                            ),
                        },
                    ),
                    explicit: false,
                    default: false,
                    origin: None,
                },
                Index {
                    name: None,
                    url: Url(
                        VerbatimUrl {
                            url: Url {
                                scheme: "https",
                                cannot_be_a_base: false,
                                username: "",
                                password: None,
                                host: Some(
                                    Domain(
                                        "pypi.python.org",
                                    ),
                                ),
                                port: None,
                                path: "/simple",
                                query: None,
                                fragment: None,
                            },
                            given: Some(
                                "https://pypi.python.org/simple",
                            ),
                        },
                    ),
                    explicit: false,
                    default: true,
                    origin: None,
                },
            ],
            flat_index: [],
            no_index: false,
        },
        python: None,
        install_mirrors: PythonInstallMirrors {
            python_install_mirror: None,
            pypy_install_mirror: None,
        },
        system: false,
        extras: None,
        break_system_packages: false,
        target: None,
        prefix: None,
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
        no_build_isolation: false,
        no_build_isolation_package: [],
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
        allow_empty_requirements: false,
        strict: false,
        dependency_mode: Transitive,
        resolution: Highest,
        prerelease: IfNecessaryOrExplicit,
        dependency_metadata: DependencyMetadata(
            {},
        ),
        output_file: None,
        no_strip_extras: false,
        no_strip_markers: false,
        no_annotate: false,
        no_header: false,
        custom_compile_command: None,
        generate_hashes: false,
        config_setting: ConfigSettings(
            {},
        ),
        python_version: None,
        python_platform: None,
        universal: false,
        exclude_newer: None,
        no_emit_package: [],
        emit_index_url: false,
        emit_find_links: false,
        emit_build_options: false,
        emit_marker_expression: false,
        emit_index_annotation: false,
        annotation_style: Split,
        link_mode: Hardlink,
        compile_bytecode: false,
        sources: Enabled,
        hash_checking: Some(
            Verify,
        ),
        upgrade: None,
        reinstall: Packages(
            [
                PackageName(
                    "XXX",
                ),
            ],
        ),
    },
}
```

---

_Comment by @charliermarsh on 2024-11-21 18:10_

Thanks. One more question: what about `uv lock --show-settings`?

---

_Comment by @zanieb on 2024-11-21 18:19_

~Could you also share verbose logs for the failure?~

Did you configure it in the `[pip]` section?

---

_Comment by @charliermarsh on 2024-11-22 03:16_

Ah yeah, that almost certainly explains it. Are you using `[pip]` in `uv.toml`? (Can you paste the contents?)

---

_Comment by @befelix on 2024-11-22 07:12_

Thanks, using `pip`-config instead of `index` was is indeed the issue. That config file is somewhat older from a time when `pip` was the only config setting possible - I completely missed that this needs to be adapted with the new index options. Thanks a lot for helping me figure this out.

---

_Closed by @befelix on 2024-11-22 07:12_

---
