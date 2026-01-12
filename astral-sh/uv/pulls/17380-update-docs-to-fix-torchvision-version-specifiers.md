```yaml
number: 17380
title: Update docs to fix torchvision version specifiers for CPU wheels
type: pull_request
state: open
author: appleparan
labels: []
assignees: []
base: main
head: appleparan/fix-pytorch-docs
created_at: 2026-01-09T14:56:30Z
updated_at: 2026-01-12T13:28:41Z
url: https://github.com/astral-sh/uv/pull/17380
synced_at: 2026-01-12T14:04:56Z
```

# Update docs to fix torchvision version specifiers for CPU wheels

---

_Pull request opened by @appleparan on 2026-01-09 14:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The CPU index provides wheels with different local version suffixes: `+cpu` for x86_64 (Linux) and AMD64 (Windows), but no suffix for aarch64 (Linux) and arm64 (macOS). Add platform-specific version specifiers to match the available wheels. 

This PR doesn't fix a uv bug, but it addresses and can close the issues raised here. #16386 #17314 


<!-- How was it tested? -->


---

_Review requested from @charliermarsh by @zanieb on 2026-01-09 15:55_

---

_Comment by @appleparan on 2026-01-10 02:57_

I've added `environments` constraint to avoid resolution conflicts between
dependencies, `sources`, and `markers`.

Without the `environments` constraint, uv generates all possible combinations of `sources` and `markers`. This causes local versions like `torchvision==0.24.1+cpu` to appear across all source indices, and produces logically impossible marker expressions such as:

```
(extra == 'cpu' and extra == 'cu126')
```

---

_@konstin reviewed on 2026-01-12 10:33_

---

_Review comment by @konstin on `docs/guides/integration/pytorch.md`:252 on 2026-01-12 10:33_

CC @atalman just to confirm that this is intentional

---

_@konstin reviewed on 2026-01-12 13:08_

---

_Review comment by @konstin on `docs/guides/integration/pytorch.md`:252 on 2026-01-12 13:08_

For context, that is the list of wheels for torch 2.9.1 on Python 3.14:
```
torch-2.9.1+cpu-cp314-cp314-manylinux_2_28_aarch64.whl
torch-2.9.1+cpu-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+cpu-cp314-cp314-win_amd64.whl
torch-2.9.1+cpu-cp314-cp314t-manylinux_2_28_aarch64.whl
torch-2.9.1+cpu-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+cpu-cp314-cp314t-win_amd64.whl
torch-2.9.1+cu126-cp314-cp314-manylinux_2_28_aarch64.whl
torch-2.9.1+cu126-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+cu126-cp314-cp314-win_amd64.whl
torch-2.9.1+cu126-cp314-cp314t-manylinux_2_28_aarch64.whl
torch-2.9.1+cu126-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+cu126-cp314-cp314t-win_amd64.whl
torch-2.9.1+cu128-cp314-cp314-manylinux_2_28_aarch64.whl
torch-2.9.1+cu128-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+cu128-cp314-cp314-win_amd64.whl
torch-2.9.1+cu128-cp314-cp314t-manylinux_2_28_aarch64.whl
torch-2.9.1+cu128-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+cu128-cp314-cp314t-win_amd64.whl
torch-2.9.1+cu129-cp314-cp314-manylinux_2_28_aarch64.whl
torch-2.9.1+cu129-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+cu129-cp314-cp314t-manylinux_2_28_aarch64.whl
torch-2.9.1+cu129-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+cu130-cp314-cp314-manylinux_2_28_aarch64.whl
torch-2.9.1+cu130-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+cu130-cp314-cp314-win_amd64.whl
torch-2.9.1+cu130-cp314-cp314t-manylinux_2_28_aarch64.whl
torch-2.9.1+cu130-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+cu130-cp314-cp314t-win_amd64.whl
torch-2.9.1+rocm6.3-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+rocm6.3-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+rocm6.4-cp314-cp314-manylinux_2_28_x86_64.whl
torch-2.9.1+rocm6.4-cp314-cp314t-manylinux_2_28_x86_64.whl
torch-2.9.1+xpu-cp314-cp314-linux_x86_64.whl
torch-2.9.1+xpu-cp314-cp314-win_amd64.whl
torch-2.9.1+xpu-cp314-cp314t-linux_x86_64.whl
torch-2.9.1+xpu-cp314-cp314t-win_amd64.whl
torch-2.9.1-cp314-cp314-macosx_11_0_arm64.whl
torch-2.9.1-cp314-cp314t-macosx_11_0_arm64.whl
```

---

_@charliermarsh reviewed on 2026-01-12 13:28_

---

_Review comment by @charliermarsh on `docs/guides/integration/pytorch.md`:252 on 2026-01-12 13:28_

I thought this was fixed in https://github.com/pytorch/test-infra/pull/7589, but perhaps there hasn't been a release since then?

---
