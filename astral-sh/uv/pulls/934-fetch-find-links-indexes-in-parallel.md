```yaml
number: 934
title: "Fetch `--find-links` indexes in parallel"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: charlie/parallel
created_at: 2024-01-15T19:07:07Z
updated_at: 2024-01-16T10:37:37Z
url: https://github.com/astral-sh/uv/pull/934
synced_at: 2026-01-10T15:39:02Z
```

# Fetch `--find-links` indexes in parallel

---

_Pull request opened by @charliermarsh on 2024-01-15 19:07_

## Summary

Removes a TODO.

## Test Plan

Tested manually with:

```shell
cargo run -p puffin-cli -- \
    pip compile requirements.in -n \
    --find-links 'https://download.pytorch.org/whl/torch_stable.html' \
    --find-links 'https://storage.googleapis.com/jax-releases/jax_cuda_releases.html' \
    --verbose
```

And inspecting the logs to ensure that the two requests were kicked off concrurently.

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 19:07_

---

_Label `performance` added by @charliermarsh on 2024-01-15 19:07_

---

_Label `internal` added by @charliermarsh on 2024-01-15 19:07_

---

_@konstin approved on 2024-01-16 10:37_

---

_Merged by @konstin on 2024-01-16 10:37_

---

_Closed by @konstin on 2024-01-16 10:37_

---

_Branch deleted on 2024-01-16 10:37_

---
