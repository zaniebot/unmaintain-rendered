```yaml
number: 17460
title: Improve create-python-mirror script
type: pull_request
state: open
author: akx
labels: []
assignees: []
base: main
head: create-python-mirror-embetter
created_at: 2026-01-14T11:53:03Z
updated_at: 2026-01-14T12:01:13Z
url: https://github.com/astral-sh/uv/pull/17460
synced_at: 2026-01-14T12:44:12Z
```

# Improve create-python-mirror script

---

_@akx_

## Summary

This PR improves the create-python-mirror script (stacked on @plucia-mitre's #16475) with a couple of extra switches that may be useful for creating limited mirrors of Python builds:

* `--dry-run`, to see what would be downloaded
* `--exclude`, to exclude e.g. `musl` or `debug` builds
* `--only-latest-per-major-minor`, to only download the latest patch version per `MAJOR.MINOR` version.

Summa summarum,

```
uv run crates/uv-python/fetch-download-metadata.py
uv run scripts/create-python-mirror.py --name cpython --arch x86_64 --os linux --version '3.1[234].\d+$' --target python-downloads --exclude='freethreaded|musl|debug' --only-latest-per-major-minor --mirror-url=http://192.168.68.112:8080/ --save-metadata
```
happily generates a directory containing only the latest CPython 3.12/.13/.14 glibc builds, and it can be used as a download depot, as provable with
```
uv run -m http.server -d python-downloads 8080
docker run -it --platform=linux/amd64 ghcr.io/astral-sh/uv:trixie-slim sh -c 'env UV_PYTHON_DOWNLOADS_JSON_URL="http://192.168.68.112:8080/download-metadata.json" uv run --python=3.13 python'
```
(assuming your machine is `192.168.68.112` on the network 😇)

## Test Plan

See above! 😄 

---

cc @MeitarR for #12838 and #17171


---
