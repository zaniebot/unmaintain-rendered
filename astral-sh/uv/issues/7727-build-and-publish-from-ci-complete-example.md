---
number: 7727
title: Build and publish from CI complete example
type: issue
state: closed
author: davidlrobinson
labels:
  - documentation
assignees: []
created_at: 2024-09-26T20:19:33Z
updated_at: 2025-09-25T19:04:06Z
url: https://github.com/astral-sh/uv/issues/7727
synced_at: 2026-01-10T01:24:18Z
---

# Build and publish from CI complete example

---

_Issue opened by @davidlrobinson on 2024-09-26 20:19_

First off, thank you for your work on uv. I read in #7475 that one of the intended uses of `uv build` is the following:

> 1. Publishing a pure Python package from CI
> - Call uv build
> - Clear the venv, install the wheel, run a smoke test
> - Clear the venv, install the source distribution, run a smoke test
> - Call uv publish

The docs mention that a built package can be tested with `uv run --with <PACKAGE> --no-project -- python -c "import <PACKAGE>"`. Is there a complete example demonstrating this in practice from CI?

---

_Label `documentation` added by @zanieb on 2024-09-26 20:25_

---

_Comment by @vtnate on 2024-09-27 16:13_

In addition, can you share an example workflow file for using this with GitHub Actions, or at least a hint? If I am already using [the recommended workflow](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/) (without the signing steps) for trusted publishing from GHA, do I just replace the `build` step internals with `uv build`? I don't think using `uv publish` is appropriate in that case, correct?

---

_Assigned to @zanieb by @zanieb on 2024-09-30 16:41_

---

_Comment by @konstin on 2024-11-03 17:47_

Here's a complete example: https://github.com/astral-sh/trusted-publishing-examples

---

_Unassigned @zanieb by @konstin on 2024-11-03 17:47_

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2024-11-03 18:09_

---

_Comment by @konstin on 2025-09-25 19:04_

Done in https://github.com/astral-sh/uv/pull/15753

---

_Closed by @konstin on 2025-09-25 19:04_

---
