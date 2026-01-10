```yaml
number: 273
title: Handle dist info casing mismatch in worker
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: Handle-dist-info-casing-mismatch-in-worker
created_at: 2023-11-01T14:58:07Z
updated_at: 2023-11-02T11:04:30Z
url: https://github.com/astral-sh/uv/pull/273
synced_at: 2026-01-10T15:50:28Z
```

# Handle dist info casing mismatch in worker

---

_Pull request opened by @konstin on 2023-11-01 14:58_

The metadata name may be uppercase, while the wheel and dist info names are lowercase, or the metadata name and the dist info name are lowercase, while the wheel name is uppercase. Either way, we just search the wheel for the name. See `find_dist_info`: https://github.com/astral-sh/puffin/blob/2652caa3e31282afc2f1e1ca581ac4f553af710d/crates/install-wheel-rs/src/wheel.rs#L1024-L1057

I tested this with `wrangler dev` and `bio_embeddings[all]`

---

_Review requested from @charliermarsh by @konstin on 2023-11-01 14:58_

---

_@charliermarsh reviewed on 2023-11-01 15:07_

---

_Review comment by @charliermarsh on `workers/pypi-metadata/src/index.ts`:83 on 2023-11-01 15:07_

I think I have Rust brain but should we save this target to a variable outside of the loop?

---

_@charliermarsh approved on 2023-11-01 15:07_

---

_@konstin reviewed on 2023-11-02 11:03_

---

_Review comment by @konstin on `workers/pypi-metadata/src/index.ts`:83 on 2023-11-02 11:03_

I really want an IDE extension that tells me what optimizations (e.g. here constant propagation) compiler or JIT see or don't see

---

_Merged by @charliermarsh on 2023-11-02 11:04_

---

_Closed by @charliermarsh on 2023-11-02 11:04_

---

_Branch deleted on 2023-11-02 11:04_

---
