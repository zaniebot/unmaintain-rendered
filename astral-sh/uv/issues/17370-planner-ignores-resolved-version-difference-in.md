```yaml
number: 17370
title: Planner ignores resolved version difference in favour of cache hit
type: issue
state: closed
author: EliteTK
labels:
  - bug
assignees: []
created_at: 2026-01-09T01:23:18Z
updated_at: 2026-01-09T20:43:21Z
url: https://github.com/astral-sh/uv/issues/17370
synced_at: 2026-01-12T16:02:49Z
```

# Planner ignores resolved version difference in favour of cache hit

---

_@EliteTK_

### Summary

Scenario:

* uv is asked to re-install a dependency with `--no-cache`
* the dependency has a dynamic version
* the installed version is out of date but the dependency's cache-keys have not changed

Due to `--no-cache` uv will call the resolver which will in turn run the necessary bits of the dependency's build system to get the actual version. The resolver will report that there is a newer version but the planner will claim the dependency is in-date due to valid cache keys (despite `--no-cache`?).

It's a bit easier to show rather than tell:

```bash
#!/usr/bin/env bash

set -e

tempdir=$(mktemp -d)
trap 'rm -rf "$tempdir"' EXIT

mkdir -p -- "$tempdir/lib"
cat <<EOF >"$tempdir/lib/pyproject.toml"
[project]
name = "lib"
dynamic = ["version", "description"]
[build-system]
requires = ["flit_core >=3.2,<4"]
build-backend = "flit_core.buildapi"
EOF
cat <<EOF >"$tempdir/lib/lib.py"
"""."""
__version__ = "0.1.0"
EOF

mkdir -p -- "$tempdir/app"
cat <<EOF >"$tempdir/app/pyproject.toml"
[project]
name = "app"
version = "0.1.0"
description = ""
dependencies = ["lib"]
[tool.uv.sources]
lib = { path = "../lib" }
EOF
touch "$tempdir/app/main.py"

set -x

uv --directory "$tempdir/app/" venv
uv --directory "$tempdir/app/" pip install .

sed -i 's/0\.1\.0/0\.1\.1/' "$tempdir/lib/lib.py"

uv -vvv --directory "$tempdir/app/" pip install --no-cache . 2>&1 | sed -n '/TRACE .* lib==0\.1\.1/,/^DEBUG Requirement already installed/p'
```

This produces:

```
+ uv --directory /tmp/tmp.XyXmVXgZ9t/app/ venv
Using CPython 3.13.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
+ uv --directory /tmp/tmp.XyXmVXgZ9t/app/ pip install .
Resolved 2 packages in 185ms
   Building lib @ file:///tmp/tmp.XyXmVXgZ9t/lib
   Building app @ file:///tmp/tmp.XyXmVXgZ9t/app
      Built lib @ file:///tmp/tmp.XyXmVXgZ9t/lib
      Built app @ file:///tmp/tmp.XyXmVXgZ9t/app
Prepared 2 packages in 505ms
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 2 packages in 0.44ms
 + app==0.1.0 (from file:///tmp/tmp.XyXmVXgZ9t/app)
 + lib==0.1.0 (from file:///tmp/tmp.XyXmVXgZ9t/lib)
+ sed -i 's/0\.1\.0/0\.1\.1/' /tmp/tmp.XyXmVXgZ9t/lib/lib.py
+ uv -vvv --directory /tmp/tmp.XyXmVXgZ9t/app/ pip install --no-cache .
+ sed -n '/TRACE .* lib==0\.1\.1/,/^DEBUG Requirement already installed/p'
TRACE Assigned packages: root==0a0.dev0, app==0.1.0, lib==0.1.1
DEBUG Tried 2 versions: app 1, lib 1
DEBUG marker environment resolution took 0.000s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.11", version: "3.13.11" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.12.63_1", platform_system: "Linux", platform_version: "#1 SMP PREEMPT_DYNAMIC Thu Dec 18 18:36:00 UTC 2025", python_full_version: StringVersion { string: "3.13.11", version: "3.13.11" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> app
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
TRACE Resolution edge: app -> lib
TRACE Resolution edge:     0.1.0 -> 0.1.1
Resolved 2 packages in 398ms
DEBUG Must revalidate requirement: app
TRACE Comparing installed with source: InstalledDist { kind: Url(InstalledDirectUrlDist { name: PackageName("lib"), version: "0.1.0", direct_url: LocalDirectory { url: "file:///tmp/tmp.XyXmVXgZ9t/lib", dir_info: DirInfo { editable: Some(false) }, subdirectory: None }, url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/tmp/tmp.XyXmVXgZ9t/lib", query: None, fragment: None }, editable: false, path: "/tmp/tmp.XyXmVXgZ9t/app/.venv/lib/python3.13/site-packages/lib-0.1.0.dist-info", cache_info: Some(CacheInfo { timestamp: Some(Timestamp(SystemTime { tv_sec: 1767921490, tv_nsec: 849237843 })), commit: None, tags: None, env: {}, directories: {"src": None} }), build_info: Some(BuildInfo { config_settings: ConfigSettings({}), extra_build_requires: [], extra_build_variables: {} }) }), metadata_cache: OnceLock(<uninit>), tags_cache: OnceLock(<uninit>) } Directory { install_path: "/tmp/tmp.XyXmVXgZ9t/lib", editable: Some(false), virtual: Some(false), url: VerbatimUrl { url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/tmp/tmp.XyXmVXgZ9t/lib", query: None, fragment: None }, given: Some("../lib") } }
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1767921490, tv_nsec: 849237843 }), None, None, {}, {"src": None}. Most recently modified: /tmp/tmp.XyXmVXgZ9t/lib/pyproject.toml
DEBUG Requirement already installed: lib==0.1.0 (from file:///tmp/tmp.XyXmVXgZ9t/lib)
+ rm -rf /tmp/tmp.XyXmVXgZ9t
```

Specifically note that the resolver reports that `lib` is now at `0.1.1` but the planner claims the cache (as in, the installed version itself) is still valid.

This is because of this specific call:

https://github.com/astral-sh/uv/blob/25d691eeafc9089a77d4fe1d82f22ca48006cda0/crates/uv-installer/src/plan.rs#L125-L135

Which passes the name of the resolved dist, and the installed dist, but not the resolved dist's version (which at this point is set).

The dynamic version + no-cache situation is contrived, but it seems like this logic might break other things. It's just not clear what it would break. It's curious why this hasn't caused any more noticeable problems so far.

It seems there _should_ be a version check if the resolved version happens to be newer. But it's also unclear if that may have unintended side effects.

Probably worth investigating further at some point.

### Platform

Linux 6.12.63_1 x86_64 GNU/Linux

### Version

uv 0.9.22

### Python version

_No response_

---

_Label `bug` added by @EliteTK on 2026-01-09 01:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 19:09_

---

_Closed by @charliermarsh on 2026-01-09 20:43_

---
