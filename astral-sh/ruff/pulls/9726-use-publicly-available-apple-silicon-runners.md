```yaml
number: 9726
title: Use publicly available Apple Silicon runners
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/silicon
created_at: 2024-01-31T00:37:00Z
updated_at: 2024-01-31T15:36:42Z
url: https://github.com/astral-sh/ruff/pull/9726
synced_at: 2026-01-12T15:55:30Z
```

# Use publicly available Apple Silicon runners

---

_@charliermarsh_

## Summary

This PR switches over to the `macos-14` runners for our macOS wheel builds, which are GitHub's newly announced public M1 macOS runners (https://github.blog/changelog/2024-01-30-github-actions-introducing-the-new-m1-macos-runner-available-to-open-source/).

Before:

- x64_64: 10m 38s (https://github.com/astral-sh/ruff/actions/runs/7703465381/job/20993903864)
- Universal: 19m 35s (https://github.com/astral-sh/ruff/actions/runs/7703465381/job/20993902533)

After:

- x64_64: 3m 30s (https://github.com/astral-sh/ruff/actions/runs/7719827902/job/21043743558?pr=9726)
- Universal: 5m 59s (https://github.com/astral-sh/ruff/actions/runs/7719827902/job/21043743243?pr=9726)

So it's like > 3x speedup for what is currently the bottleneck in our release pipeline.


---

_Marked ready for review by @charliermarsh on 2024-01-31 01:24_

---

_Review requested from @konstin by @charliermarsh on 2024-01-31 01:31_

---

_Label `release` added by @charliermarsh on 2024-01-31 01:31_

---

_@charliermarsh reviewed on 2024-01-31 01:31_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:81 on 2024-01-31 01:31_

Unfortunately we can no longer run the x64_64 wheel (since this is an ARM runner, we're cross-compiling), but I think it's fine and worth it (we already run this test in the Universal branch below).

---

_@messense reviewed on 2024-01-31 01:47_

---

_Review comment by @messense on `.github/workflows/release.yaml`:81 on 2024-01-31 01:47_

IMO if the M1 runner has Rostta2 enabled it should run fine? At least for `ruff --help`.

---

_@charliermarsh reviewed on 2024-01-31 02:24_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:81 on 2024-01-31 02:24_

Lemme look back at what failed...

---

_@charliermarsh reviewed on 2024-01-31 02:24_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:81 on 2024-01-31 02:24_

I think it was the `pip install dist/${{ env.PACKAGE_NAME }}-*.whl --force-reinstall`, saying that the wheel isn't supported for the current platform.

---

_@messense reviewed on 2024-01-31 02:27_

---

_Review comment by @messense on `.github/workflows/release.yaml`:81 on 2024-01-31 02:27_

Oh right, the python interpreter it installed might not be universal2, if it is you can probably use `arch -x86_64 python` to switch to x86_64 mode.

Anyway it's definitly not a big issue.

---

_@konstin approved on 2024-01-31 12:37_

:rocket: 

---

_@charliermarsh reviewed on 2024-01-31 15:36_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:81 on 2024-01-31 15:36_

I appreciate you chiming in, you're always so helpful.

---

_Merged by @charliermarsh on 2024-01-31 15:36_

---

_Closed by @charliermarsh on 2024-01-31 15:36_

---

_Branch deleted on 2024-01-31 15:36_

---
