```yaml
number: 961
title: Add release workflow
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/release
created_at: 2024-01-18T05:10:24Z
updated_at: 2024-01-18T20:44:12Z
url: https://github.com/astral-sh/uv/pull/961
synced_at: 2026-01-12T16:04:19Z
```

# Add release workflow

---

_@charliermarsh_

## Summary

This PR adds a release workflow powered by `cargo-dist`. It's similar to the version that's PR'd in Ruff (https://github.com/astral-sh/ruff/pull/9559), with the exception that it doesn't include the Docker build or the "update dependents" step for pre-commit.


---

_@charliermarsh reviewed on 2024-01-18 05:10_

---

_Review comment by @charliermarsh on `pyproject.toml`:6 on 2024-01-18 05:10_

So this would publish a binary named `puffin` to the PyPI package `puffin-alpha`. (`puffin` is taken, and it doesn't really matter what we use in the interim.)

---

_@charliermarsh reviewed on 2024-01-18 05:11_

---

_Review comment by @charliermarsh on `.github/workflows/release.yml`:1 on 2024-01-18 05:11_

(Generated file.)

---

_@charliermarsh reviewed on 2024-01-18 05:11_

---

_Review comment by @charliermarsh on `pyproject.toml`:14 on 2024-01-18 05:11_

If we want to publish this (stealthily) to PyPI, we need to remove the URLs and authors from the `Cargo.toml` (they're picked up by Maturin).

---

_Comment by @charliermarsh on 2024-01-18 05:14_

I created an alt for PyPI to publish under: https://pypi.org/user/whiskeyjack/. Do folks think it's worth publishing? I suspect it is.

---

_@charliermarsh reviewed on 2024-01-18 05:16_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:48 on 2024-01-18 05:16_

Intentionally omitting the README for stealthy PyPI.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-18 05:16_

---

_Review requested from @konstin by @charliermarsh on 2024-01-18 05:16_

---

_Label `internal` added by @charliermarsh on 2024-01-18 05:16_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:20 on 2024-01-18 09:53_

(same as in ruff)

```suggestion
      - .github/workflows/*.yaml
```

---

_@konstin approved on 2024-01-18 09:53_

---

_Comment by @zanieb on 2024-01-18 14:47_

Sounds great to publish

---

_Merged by @charliermarsh on 2024-01-18 20:44_

---

_Closed by @charliermarsh on 2024-01-18 20:44_

---

_Branch deleted on 2024-01-18 20:44_

---
