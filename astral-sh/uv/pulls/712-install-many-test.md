```yaml
number: 712
title: Install many test
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: konsti/pypi_10k_most_dependents
head: konsti/install-many
created_at: 2023-12-20T13:22:18Z
updated_at: 2023-12-24T18:20:48Z
url: https://github.com/astral-sh/uv/pull/712
synced_at: 2026-01-10T15:44:44Z
```

# Install many test

---

_Pull request opened by @konstin on 2023-12-20 13:22_

This script can install a large set of packages in batches of 100, ignoring build failures and missing wheels.

It has already found a crash (to be investigated), so i say it's ready to merge.

```
puffin-dev failed
  Caused by: Failed to install
  Caused by: Failed to install: mkl-fft==1.3.6
  Caused by: The wheel is invalid: Invalid Wheel-Version in WHEEL file
```

Example:

```
virtualenv --clear -p 3.12 .venv312 -q && VIRTUAL_ENV=.venv312 RUST_LOG=puffin=info cargo run --bin puffin-dev --release -- install-many --no-build scripts/popular_packages/pypi_100_most_dependents.txt
    Finished release [optimized] target(s) in 0.08s
     Running `target/release/puffin-dev install-many --no-build scripts/popular_packages/pypi_100_most_dependents.txt`
2023-12-20T13:19:54.866060Z  INFO Got 100 requirements
2023-12-20T13:19:54.966808Z  INFO Failed to find wheel: Failed to find a version of torch that satisfies the requirement
2023-12-20T13:19:54.966822Z  INFO Failed to find 1 wheel(s)
2023-12-20T13:19:54.966833Z  INFO Removed 6 source dists
2023-12-20T13:19:54.967735Z  INFO Cached: 93, Uncached 0
2023-12-20T13:19:54.967745Z  INFO Failed to fetch 0 wheel(s)
2023-12-20T13:19:55.017639Z  INFO Installed 93 wheels
```

---

_Marked ready for review by @konstin on 2023-12-21 12:23_

---

_@zanieb approved on 2023-12-21 14:41_

---

_@charliermarsh approved on 2023-12-24 18:19_

---

_Label `internal` added by @charliermarsh on 2023-12-24 18:20_

---

_Merged by @charliermarsh on 2023-12-24 18:20_

---

_Closed by @charliermarsh on 2023-12-24 18:20_

---

_Branch deleted on 2023-12-24 18:20_

---
