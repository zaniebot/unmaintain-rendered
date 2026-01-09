---
number: 8162
title: "how to specify build version with `uv build`?"
type: issue
state: closed
author: struckchure
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-13T23:05:27Z
updated_at: 2024-10-14T16:30:43Z
url: https://github.com/astral-sh/uv/issues/8162
synced_at: 2026-01-07T13:12:17-06:00
---

# how to specify build version with `uv build`?

---

_Issue opened by @struckchure on 2024-10-13 23:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm building a Python package, and I need to add a release workflow, with a version tag too.

```yaml
name: Publish to PyPI

on:
  push:
    tags:
      - "v*"

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Install uv
        uses: astral-sh/setup-uv@v3

      - name: Set up Python
        run: uv python install

      - name: Install dependencies
        run: uv sync

      - name: Build and publish
        run: |
          uv build
          uv publish --token ${{ secrets.PYPI_API_TOKEN }}
```

I basically need the equivalent of this, `poetry version $(git describe --tags --abbrev=0)`

---

_Comment by @charliermarsh on 2024-10-14 13:44_

Like, you need to extract the current project version into an environment variable?

---

_Label `question` added by @charliermarsh on 2024-10-14 13:44_

---

_Comment by @struckchure on 2024-10-14 16:28_

> Like, you need to extract the current project version into an environment variable?

I need to set a build version for my package

---

_Comment by @zanieb on 2024-10-14 16:30_

You're looking for https://github.com/astral-sh/uv/issues/6298, we don't support this yet but you can track it there.

---

_Closed by @zanieb on 2024-10-14 16:30_

---

_Label `duplicate` added by @zanieb on 2024-10-14 16:30_

---
