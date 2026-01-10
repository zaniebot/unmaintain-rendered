```yaml
number: 501
title: Check compatibility for cached unzipped wheels
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: unzipped-wheel-cache-tags-registry
created_at: 2023-11-26T15:56:38Z
updated_at: 2023-11-28T00:03:59Z
url: https://github.com/astral-sh/uv/pull/501
synced_at: 2026-01-10T15:44:44Z
```

# Check compatibility for cached unzipped wheels

---

_Pull request opened by @konstin on 2023-11-26 15:56_

**Motivation** Previously, we would install any wheel with the correct package name and version from the cache, even if it doesn't match the current python interpreter.

**Summary** The unzipped wheel cache for registries now uses the entire wheel filename over the name-version (`editables-0.5-py3-none-any.whl` over `editables-0.5`).

Built wheels are not stored in the `wheels-v0` unzipped wheels cache anymore. For each source distribution, there can be multiple built wheels (with different compatibility tags), so i argue that we need a different cache structure for them (follow up PR).

For `all-kinds.in` with

```bash
rm -rf cache-all-kinds
virtualenv --clear -p 3.12 .venv
cargo run --bin puffin -- pip-sync --cache-dir cache-all-kinds target/all-kinds.txt
```

we get:

**Before**
```
cache-all-kinds/wheels-v0/
├── registry
│   ├── annotated_types-0.6.0
│   ├── asgiref-3.7.2
│   ├── blinker-1.7.0
│   ├── certifi-2023.11.17
│   ├── cffi-1.16.0
│   ├── [...]
│   ├── tzdata-2023.3
│   ├── urllib3-2.1.0
│   └── wheel-0.42.0
└── url
    ├── 4b8be67c801a7ecb
    │   ├── flask
    │   └── flask-3.0.0.dist-info
    ├── 6781bd6440ae72c2
    │   ├── werkzeug
    │   └── werkzeug-3.0.1.dist-info
    └── a67db8ed076e3814
        ├── pydantic_extra_types
        └── pydantic_extra_types-2.1.0.dist-info

48 directories, 0 files
```

**After**

```
cache-all-kinds/wheels-v0/
├── registry
│   ├── annotated_types-0.6.0-py3-none-any.whl
│   ├── asgiref-3.7.2-py3-none-any.whl
│   ├── blinker-1.7.0-py3-none-any.whl
│   ├── certifi-2023.11.17-py3-none-any.whl
│   ├── cffi-1.16.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
│   ├── [...]
│   ├── tzdata-2023.3-py2.py3-none-any.whl
│   ├── urllib3-2.1.0-py3-none-any.whl
│   └── wheel-0.42.0-py3-none-any.whl
└── url
    └── 4b8be67c801a7ecb
        └── flask-3.0.0-py3-none-any.whl

39 directories, 0 files
```

**Outlook** Part of #477 "Fix wheel caching". Further tasks:
* Replace the `CacheShard` with `WheelMetadataCache` which handles urls properly.
* Delete unzipped wheels when their remote wheel changed
* Store built wheels next to the `metadata.json` in the source dist directory; delete built wheels when their source dist changed (different cache bucket, but it's the same problem of fixing wheel caching) I'll make stacked PRs for those

---

_Review comment by @konstin on `crates/puffin-installer/src/cache.rs`:35 on 2023-11-26 15:57_

This is stop gap just to avoid a regression, it'll be rendered unnecessary by using `WheelMetadataCache`

---

_@konstin reviewed on 2023-11-26 15:57_

---

_Converted to draft by @konstin on 2023-11-26 15:59_

---

_Marked ready for review by @konstin on 2023-11-26 16:03_

---

_@charliermarsh approved on 2023-11-27 19:30_

---

_Merged by @charliermarsh on 2023-11-28 00:03_

---

_Closed by @charliermarsh on 2023-11-28 00:03_

---

_Branch deleted on 2023-11-28 00:03_

---
