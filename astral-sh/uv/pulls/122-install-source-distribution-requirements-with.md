```yaml
number: 122
title: Install source distribution requirements with puffin itself instead of pip
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: build-with-puffin-install
created_at: 2023-10-18T19:06:51Z
updated_at: 2023-10-18T19:11:18Z
url: https://github.com/astral-sh/uv/pull/122
synced_at: 2026-01-10T15:50:28Z
```

# Install source distribution requirements with puffin itself instead of pip

---

_Pull request opened by @konstin on 2023-10-18 19:06_

This is also a lot faster. Unfortunately it copies a lot of code from the sync cli since the `Printer` is private.

The first commit are some refactorings i made when i thought about how i could reuse the existing code.

---

_Comment by @konstin on 2023-10-18 19:08_

Perf numbers:
```console
$ /home/konsti/projects/puffin/crates/puffin-build/test.sh
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `/home/konsti/projects/puffin/target/debug/puffin-build --wheels wheels downloads/tqdm-4.66.1.tar.gz`
2023-10-18T19:08:22.153016Z DEBUG puffin_build: Unpacking for build downloads/tqdm-4.66.1.tar.gz
2023-10-18T19:08:22.159925Z  INFO extract_archive: puffin_build: close time.busy=6.85ms time.idle=7.50µs
2023-10-18T19:08:22.163314Z DEBUG resolve_and_install: puffin_build: Installing 3 build requirements
2023-10-18T19:08:22.200051Z  INFO resolve_and_install: puffin_build: close time.busy=36.7ms time.idle=59.7µs
2023-10-18T19:08:22.200079Z DEBUG puffin_build: Calling `setuptools.build_meta.get_requires_for_build_wheel()`
2023-10-18T19:08:22.426338Z  INFO run_python_script{python_interpreter="/tmp/.tmpTzccP5/.venv/bin/python"}: puffin_build: close time.busy=226ms time.idle=6.10µs
2023-10-18T19:08:22.426444Z DEBUG build{wheel_dir="wheels"}: puffin_build: Calling `setuptools.build_meta.build_wheel(metadata_directory=None)`
2023-10-18T19:08:22.537135Z  INFO build{wheel_dir="wheels"}:run_python_script{python_interpreter="/tmp/.tmpTzccP5/.venv/bin/python"}: puffin_build: close time.busy=111ms time.idle=5.50µs
2023-10-18T19:08:22.537191Z  INFO build{wheel_dir="wheels"}: puffin_build: close time.busy=111ms time.idle=3.34µs
Wheel built to /home/konsti/projects/puffin/crates/puffin-build/wheels/tqdm-4.66.1-py3-none-any.whl
2023-10-18T19:08:22.540709Z DEBUG puffin_build: Took 388ms
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `/home/konsti/projects/puffin/target/debug/puffin-build --wheels wheels downloads/geoextract-0.3.1.tar.gz`
2023-10-18T19:08:22.632965Z DEBUG puffin_build: Unpacking for build downloads/geoextract-0.3.1.tar.gz
2023-10-18T19:08:22.634100Z  INFO extract_archive: puffin_build: close time.busy=1.08ms time.idle=6.77µs
2023-10-18T19:08:22.634362Z DEBUG resolve_and_install: puffin_build: Installing 3 build requirements
2023-10-18T19:08:22.692932Z  INFO resolve_and_install: puffin_build: close time.busy=58.5ms time.idle=49.8µs
2023-10-18T19:08:22.835477Z  INFO build{wheel_dir="wheels"}: puffin_build: close time.busy=142ms time.idle=8.86µs
Wheel built to /home/konsti/projects/puffin/crates/puffin-build/wheels/geoextract-0.3.1-py3-none-any.whl
2023-10-18T19:08:22.841334Z DEBUG puffin_build: Took 208ms
```

---

_Merged by @konstin on 2023-10-18 19:11_

---

_Closed by @konstin on 2023-10-18 19:11_

---

_Branch deleted on 2023-10-18 19:11_

---
