```yaml
number: 104
title: Add basic sdist builder
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: sdist-buillder
created_at: 2023-10-16T12:34:40Z
updated_at: 2023-10-16T12:43:33Z
url: https://github.com/astral-sh/uv/pull/104
synced_at: 2026-01-10T15:50:28Z
```

# Add basic sdist builder

---

_Pull request opened by @konstin on 2023-10-16 12:34_

This adds a basic sdist builder that has been tested with two source distributions, one with a PEP 517 backend and one with setup.py.

It uses pip for requirements installation atm, lacks testing in all directions, lacks checks for recursive requirements, can't pass in already resolved versions, doesn't support prepare metadata for build to allow resolution to continue without doing the actual (native) build, error messages are mediocre, etc.

```console
$ RUST_LOG=puffin_build=debug puffin-build --wheels wheels downloads/tqdm-4.66.1.tar.gz
2023-10-16T12:28:35.503182Z DEBUG build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: Building downloads/tqdm-4.66.1.tar.gz
2023-10-16T12:28:35.521780Z  INFO build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}:extract_archive: puffin_build: close time.busy=18.4ms time.idle=16.7µs
2023-10-16T12:28:35.845096Z DEBUG build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}:resolve_and_install: puffin_build: Calling pip to install build dependencies
2023-10-16T12:28:37.668660Z  INFO build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}:resolve_and_install: puffin_build: close time.busy=1.82s time.idle=13.2µs
2023-10-16T12:28:37.668744Z DEBUG build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: Calling `setuptools.build_meta.get_requires_for_build_wheel()`
2023-10-16T12:28:38.159205Z  INFO build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}:run_python_script{python_interpreter="/tmp/.tmpm4cTra/venv/bin/python"}: puffin_build: close time.busy=490ms time.idle=13.0µs
2023-10-16T12:28:38.159304Z DEBUG build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: Calling `setuptools.build_meta.build_wheel()`
2023-10-16T12:28:38.501732Z  INFO build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}:run_python_script{python_interpreter="/tmp/.tmpm4cTra/venv/bin/python"}: puffin_build: close time.busy=342ms time.idle=15.2µs
2023-10-16T12:28:38.522700Z  INFO build_sdist{path="downloads/tqdm-4.66.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: close time.busy=3.02s time.idle=16.2µs
Wheel built to /home/konsti/projects/puffin/crates/puffin-build/wheels/tqdm-4.66.1-py3-none-any.whl
2023-10-16T12:28:38.522772Z DEBUG puffin_build: Took 3020ms
$ puffin-build --wheels wheels downloads/geoextract-0.3.1.tar.gz
2023-10-16T12:28:40.884622Z DEBUG build_sdist{path="downloads/geoextract-0.3.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: Building downloads/geoextract-0.3.1.tar.gz
2023-10-16T12:28:40.887743Z  INFO build_sdist{path="downloads/geoextract-0.3.1.tar.gz" base_python="/usr/bin/python3"}:extract_archive: puffin_build: close time.busy=2.97ms time.idle=12.6µs
2023-10-16T12:28:41.469738Z  INFO build_sdist{path="downloads/geoextract-0.3.1.tar.gz" base_python="/usr/bin/python3"}: puffin_build: close time.busy=585ms time.idle=15.3µs
Wheel built to /home/konsti/projects/puffin/crates/puffin-build/wheels/geoextract-0.3.1-py3-none-any.whl
2023-10-16T12:28:41.469814Z DEBUG puffin_build: Took 585ms
```

---

_Merged by @konstin on 2023-10-16 12:43_

---

_Closed by @konstin on 2023-10-16 12:43_

---

_Branch deleted on 2023-10-16 12:43_

---
