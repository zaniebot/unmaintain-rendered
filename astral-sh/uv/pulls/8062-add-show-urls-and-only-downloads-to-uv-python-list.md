```yaml
number: 8062
title: "Add `--show-urls` and `--only-downloads` to `uv python list`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/download-list
created_at: 2024-10-10T01:01:54Z
updated_at: 2024-12-10T18:52:41Z
url: https://github.com/astral-sh/uv/pull/8062
synced_at: 2026-01-10T11:59:59Z
```

# Add `--show-urls` and `--only-downloads` to `uv python list`

---

_Pull request opened by @zanieb on 2024-10-10 01:01_

These are useful for creating a mirror of the Python downloads for a given uv version, e.g.:

```
❯ cargo run -q -- python list --show-urls --only-downloads
cpython-3.13.0-macos-aarch64-none     https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.13.0%2B20241008-aarch64-apple-darwin-install_only_stripped.tar.gz
cpython-3.12.7-macos-aarch64-none     https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.12.7%2B20241008-aarch64-apple-darwin-install_only_stripped.tar.gz
cpython-3.11.10-macos-aarch64-none    https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.11.10%2B20241008-aarch64-apple-darwin-install_only_stripped.tar.gz
cpython-3.10.15-macos-aarch64-none    https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.10.15%2B20241008-aarch64-apple-darwin-install_only_stripped.tar.gz
cpython-3.9.20-macos-aarch64-none     https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.9.20%2B20241008-aarch64-apple-darwin-install_only_stripped.tar.gz
cpython-3.8.20-macos-aarch64-none     https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-aarch64-apple-darwin-install_only_stripped.tar.gz
pypy-3.10.14-macos-aarch64-none       https://downloads.python.org/pypy/pypy3.10-v7.3.17-macos_arm64.tar.bz2
pypy-3.9.19-macos-aarch64-none        https://downloads.python.org/pypy/pypy3.9-v7.3.16-macos_arm64.tar.bz2
pypy-3.8.16-macos-aarch64-none        https://downloads.python.org/pypy/pypy3.8-v7.3.11-macos_arm64.tar.bz2
```


---

_Label `enhancement` added by @zanieb on 2024-10-10 01:01_

---

_Comment by @zanieb on 2024-10-10 01:03_

I might add some tests for this, but they'll be kind of annoying to update.

---

_@charliermarsh approved on 2024-11-06 22:11_

---

_Merged by @zanieb on 2024-12-10 18:52_

---

_Closed by @zanieb on 2024-12-10 18:52_

---

_Branch deleted on 2024-12-10 18:52_

---
