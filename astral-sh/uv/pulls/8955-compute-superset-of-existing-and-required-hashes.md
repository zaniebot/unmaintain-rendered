```yaml
number: 8955
title: Compute superset of existing and required hashes when healing cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/hea
created_at: 2024-11-08T19:59:55Z
updated_at: 2024-11-08T20:12:31Z
url: https://github.com/astral-sh/uv/pull/8955
synced_at: 2026-01-12T16:08:34Z
```

# Compute superset of existing and required hashes when healing cache

---

_@charliermarsh_

## Summary

The basic issue here is that `uv add` will compute and store a hash for each package. But if you later run `uv pip install` _after_ `uv cache prune --ci`, we need to re-download the source distribution. After re-downloading, we compare the hashes before and after. But `uv pip install` doesn't compute any hashes by default. So the hashes "differ" and we error.

Instead, we need to compute a superset of the already-existing and newly-requested hashes when performing this re-download. (In practice, this will always be SHA-256.)

Closes https://github.com/astral-sh/uv/issues/8929.

## Test Plan

```shell
export UV_CACHE_DIR="$PWD/cache"

rm -rf "$UV_CACHE_DIR" .venv .venv-2 pyproject.toml uv.lock

echo $(uv --version)

uv init --name uv-cache-issue
cargo run add --python 3.13 "pycairo"

uv cache prune --ci

rm -rf .venv .venv-2

uv venv --python python3.11 .venv-2
. .venv-2/bin/activate
cargo run pip install "pycairo"
```


---

_Label `bug` added by @charliermarsh on 2024-11-08 20:01_

---

_Label `cache` added by @charliermarsh on 2024-11-08 20:01_

---

_Comment by @charliermarsh on 2024-11-08 20:05_

I might be able to add an automated test for this, will try...

---

_Merged by @charliermarsh on 2024-11-08 20:12_

---

_Closed by @charliermarsh on 2024-11-08 20:12_

---

_Branch deleted on 2024-11-08 20:12_

---

_Comment by @charliermarsh on 2024-11-08 20:12_

Mm hard because you need to build a wheel from source that isn't pure Python...

---
