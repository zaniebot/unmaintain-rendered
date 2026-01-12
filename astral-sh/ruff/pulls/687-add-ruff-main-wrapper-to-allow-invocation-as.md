```yaml
number: 687
title: Add ruff.__main__ wrapper to allow invocation as ‘python -m ruff’
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: __main__
created_at: 2022-11-11T20:26:51Z
updated_at: 2022-11-11T20:54:00Z
url: https://github.com/astral-sh/ruff/pull/687
synced_at: 2026-01-12T15:55:05Z
```

# Add ruff.__main__ wrapper to allow invocation as ‘python -m ruff’

---

_@andersk_

Fixes #658.

---

_@andersk reviewed on 2022-11-11 20:37_

---

_Review comment by @andersk on `ruff/__main__.py`:7 on 2022-11-11 20:37_

(Changed from `os.execv` because that seems to put the process in the background on Windows: python/cpython#53394.)

---

_@charliermarsh reviewed on 2022-11-11 20:46_

---

_Review comment by @charliermarsh on `ruff/__main__.py`:6 on 2022-11-11 20:46_

Do we need this logic from [maturin](https://github.com/PyO3/maturin/blob/main/maturin/__main__.py#L8), or can it intentionally be omitted?

---

_@andersk reviewed on 2022-11-11 20:52_

---

_Review comment by @andersk on `ruff/__main__.py`:6 on 2022-11-11 20:52_

I looked at the maturin logic, but it seems unnecessarily complicated. It finds all possible `scripts` paths via all possible `sysconfig` schemes (not just the current one), recurses through all files in all subfolders of these `scripts` paths, and strips off all file extensions to check the basename. It originated from a Windows user at PyO3/maturin#1038. But I’ve tested this simple version to work on Windows, so I don’t see why any of that complexity should be needed. Perhaps that user had a weirdly misconfigured Python installation.

---

_@charliermarsh reviewed on 2022-11-11 20:53_

---

_Review comment by @charliermarsh on `ruff/__main__.py`:6 on 2022-11-11 20:53_

Sounds good :)

---

_Merged by @charliermarsh on 2022-11-11 20:53_

---

_Closed by @charliermarsh on 2022-11-11 20:53_

---

_Branch deleted on 2022-11-11 20:54_

---
