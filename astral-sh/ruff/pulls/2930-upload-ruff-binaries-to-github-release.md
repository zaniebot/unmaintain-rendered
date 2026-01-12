```yaml
number: 2930
title: "Upload `ruff` binaries to GitHub release"
type: pull_request
state: merged
author: messense
labels:
  - release
assignees: []
merged: true
base: main
head: ruff-bin
created_at: 2023-02-15T16:52:17Z
updated_at: 2023-02-15T17:17:51Z
url: https://github.com/astral-sh/ruff/pull/2930
synced_at: 2026-01-12T04:39:44Z
```

# Upload `ruff` binaries to GitHub release

---

_Pull request opened by @messense on 2023-02-15 16:52_

Closes #2330
Closes #2355

---

_Comment by @messense on 2023-02-15 16:52_

Example run: https://github.com/messense/ruff/actions/runs/4185930263

Only the last `release` step isn't tested yet.

---

_Comment by @messense on 2023-02-15 16:54_

BTW, added the `aarch64-pc-windows-msvc` target.

---

_Comment by @charliermarsh on 2023-02-15 16:55_

Amazing!

---

_Label `release` added by @charliermarsh on 2023-02-15 16:55_

---

_Merged by @charliermarsh on 2023-02-15 17:07_

---

_Closed by @charliermarsh on 2023-02-15 17:07_

---

_Branch deleted on 2023-02-15 17:08_

---

_Review comment by @charliermarsh on `.github/workflows/ruff.yaml`:83 on 2023-02-15 17:09_

Do we need to strip the release binary, like in `ripgrep`?

https://github.com/BurntSushi/ripgrep/blob/44fb9fce2c1ee1a86c450702ea3ca2952bf6c5a7/.github/workflows/release.yml#L137

---

_@charliermarsh reviewed on 2023-02-15 17:09_

---

_@messense reviewed on 2023-02-15 17:15_

---

_Review comment by @messense on `.github/workflows/ruff.yaml`:83 on 2023-02-15 17:15_

The binary isn't that big, I wouldn't bother strip it just yet, besides strip symbols makes panic backtrace hard to read.

I'd use `maturin build --strip` to also strip the binary in wheels if we want to strip the release binaries.

---

_@charliermarsh reviewed on 2023-02-15 17:16_

---

_Review comment by @charliermarsh on `.github/workflows/ruff.yaml`:83 on 2023-02-15 17:16_

Okay cool, sounds good -- I wasn't sure.

---

_Review comment by @messense on `.github/workflows/ruff.yaml`:83 on 2023-02-15 17:17_

https://github.com/charliermarsh/ruff/blob/d8e709648d2fe7c2bd7cf867fda1918bf6f092dc/pyproject.toml#L47

Actually it's already using `maturin build --strip`.

---

_@messense reviewed on 2023-02-15 17:17_

---
