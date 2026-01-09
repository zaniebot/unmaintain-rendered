---
number: 11475
title: Support for Retrieving Project Version (Equivalent to poetry version)
type: issue
state: closed
author: shoaibkhanz
labels:
  - question
assignees: []
created_at: 2025-02-13T11:59:25Z
updated_at: 2025-02-13T14:47:53Z
url: https://github.com/astral-sh/uv/issues/11475
synced_at: 2026-01-07T13:12:18-06:00
---

# Support for Retrieving Project Version (Equivalent to poetry version)

---

_Issue opened by @shoaibkhanz on 2025-02-13 11:59_

### Question

Currently, `uv version` does not return the project version but instead returns the version of UV itself. In Poetry, we can use `poetry version` to retrieve the project’s version as defined in` pyproject.toml.` This is useful for dynamically tagging builds, such as appending dev versions in CI with `git rev-parse --short $GITHUB_SHA`.

here is an example one that is used with poetry, can something similar be achieved with uv or planned feature.

```
publish:
      runs-on: ubuntu-latest
      needs: test
      steps:
        - uses: actions/checkout@v2
        - name: Set up Python
          uses: actions/setup-python@v2
          with:
            python-version: '3.12'
        - name: Install Poetry
          run: |
            pip install poetry
        - name: Snapshot Version
          if: ${{ github.ref_type == 'branch' }}
          run: |
            VERSION=$(poetry version --short)
            poetry version "${VERSION}.dev0+$(git rev-parse --short "$GITHUB_SHA")"
        - name: Build and Publish Release
          env:
            POETRY_HTTP_BASIC_NEXUS_UPLOAD_USERNAME: ${{ secrets.NEXUS_USERNAME }}
            POETRY_HTTP_BASIC_NEXUS_UPLOAD_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
          run: |
            poetry config repositories.nexus-upload https://nexus.abc.com/repository/pypi/
            poetry publish --build --repository="nexus-upload"
```

### Platform

macos (Darwin 24.3.0 arm64)

### Version

uv 0.5.29 (Homebrew 2025-02-06)

---

_Label `question` added by @shoaibkhanz on 2025-02-13 11:59_

---

_Comment by @konstin on 2025-02-13 14:23_

Is this covered by #6298?

---

_Comment by @charliermarsh on 2025-02-13 14:47_

I believe so, yeah.

---

_Closed by @charliermarsh on 2025-02-13 14:47_

---
