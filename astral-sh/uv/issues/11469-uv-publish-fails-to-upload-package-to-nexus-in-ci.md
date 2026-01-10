---
number: 11469
title: uv publish fails to upload package to nexus in CI but is able to push to nexus locally.
type: issue
state: closed
author: shoaibkhanz
labels:
  - question
assignees: []
created_at: 2025-02-13T02:02:01Z
updated_at: 2025-02-13T11:39:12Z
url: https://github.com/astral-sh/uv/issues/11469
synced_at: 2026-01-10T01:25:06Z
---

# uv publish fails to upload package to nexus in CI but is able to push to nexus locally.

---

_Issue opened by @shoaibkhanz on 2025-02-13 02:02_

### Question

I am in the process of migrating over to uv from poetry. The last piece I am testing is publishing packages. My current issue is that while I am able to publish to nexus (uses the index from pyproject.toml) when I am testing locally but fails to do so during CI (github actions), now all permissions are set and I can able to push packages using poetry, so its some issue with CI permission in general.

Below is snippet of GH action job and the associated pyproject.toml
```
  publish:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.13'

      - name: Install uv
        uses: astral-sh/setup-uv@v5

      - name: Build and Publish Release
        env:

          UV_PUBLISH_USERNAME: ${{ secrets.NEXUS_USERNAME}}
          UV_PUBLISH_PASSWORD: ${{ secrets.NEXUS_PASSWORD}}
        run: |
          uv build --no-sources
          uv publish --index nexus
```

here is the snippet from my pyproject.toml, I expect uv publish to respect my publish index within pyproject.toml even in CI.

```
[[tool.uv.index]]
name = "nexus"
url = "https://nexus.abc.com/repository/pypi-all/simple"
publish-url = "https://nexus.abc.com/repository/pypi/"
default = true # exclude PyPI from the list of index

```

So my key question is why does it fail to publish when it can locally, I did try passing --publish-url but that hasnt worked. Any help would be appreciated. Please let me know if further details are needed.

### Platform

macos (Darwin 24.3.0 arm64)

### Version

 uv 0.5.29 (Homebrew 2025-02-06)

---

_Label `question` added by @shoaibkhanz on 2025-02-13 02:02_

---

_Renamed from "uv publish fails to upload package to nexus in CI but is successful locally." to "uv publish fails to upload package to nexus in CI but is able to push to nexus from locally." by @shoaibkhanz on 2025-02-13 02:03_

---

_Renamed from "uv publish fails to upload package to nexus in CI but is able to push to nexus from locally." to "uv publish fails to upload package to nexus in CI but is able to push to nexus locally." by @shoaibkhanz on 2025-02-13 02:03_

---

_Comment by @charliermarsh on 2025-02-13 02:05_

Does the error message include any additional info? (And have you verified that the secrets are being set / propagated correctly in your CI pipeline? Like, that `UV_PUBLISH_USERNAME` is non-empty?)

---

_Comment by @shoaibkhanz on 2025-02-13 02:18_

> Does the error message include any additional info? (And have you verified that the secrets are being set / propagated correctly in your CI pipeline? Like, that `UV_PUBLISH_USERNAME` is non-empty?)

yes the username and password, seem to work as expected, to test it I also ran the publish using poetry to make sure I was able to publish using the same secrets, I could.

```
here are the logs, 
2025-02-13T01:39:44.3357526Z   UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
2025-02-13T01:39:44.3358110Z   UV_PUBLISH_URL: https://nexus.abc.com/repository/pypi/
2025-02-13T01:39:44.3358875Z   UV_PUBLISH_USERNAME: ***
2025-02-13T01:39:44.3359243Z   UV_PUBLISH_PASSWORD: ***
2025-02-13T01:39:44.3359558Z ##[endgroup]
2025-02-13T01:39:44.3985740Z Building source distribution...
2025-02-13T01:39:44.7840907Z   × Failed to build `/home/runner/work/project/project`
2025-02-13T01:39:44.7857138Z   ├─▶ Failed to resolve requirements from `build-system.requires`
2025-02-13T01:39:44.7857957Z   ├─▶ No solution found when resolving: `hatchling`
2025-02-13T01:39:44.7860102Z   ╰─▶ Because hatchling was not found in the package registry and you require
2025-02-13T01:39:44.7861886Z       hatchling, we can conclude that your requirements are unsatisfiable.
2025-02-13T01:39:44.7863064Z 
2025-02-13T01:39:44.7863815Z       hint: An index URL (https://nexus.abc.com/repository/pypi-all/simple)
2025-02-13T01:39:44.7866345Z       could not be queried due to a lack of valid authentication credentials
2025-02-13T01:39:44.7866950Z       (401 Unauthorized).
```

---

_Comment by @shoaibkhanz on 2025-02-13 02:20_

Just so you have the full CI here, it is 

```
name: Test and Publish

on:
  pull_request:
  release:
    types:
      - published

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python_version: [ '3.13' ]
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python_version }}

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        # with:
        #   python-version: ${{ matrix.python_version }}

      - name: Enable Caching
        uses: astral-sh/setup-uv@v5
        with:
          enable-cache: true
          cache-dependency-glob: "uv.lock"

      - name: Install Python Dependencies
        if: steps.setup-uv.outputs.cache-hit != 'true'
        env:
          UV_INDEX_NEXUS_USERNAME: ${{ secrets.NEXUS_USERNAME }}
          UV_INDEX_NEXUS_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
        run: |
          uv sync

      # - name: Github Payload # for debugging
      #   run: echo "payload:\n${PAYLOAD}\n"
      #   env:
      #     PAYLOAD: ${{ toJSON(github.event) }}

      - name: Test
        run: |
          uv run task test

      - name: Minimize uv cache
        run: uv cache prune --ci

  publish:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.13'

      - name: Install uv
        uses: astral-sh/setup-uv@v5

      - name: Build and Publish Release
        env:

          UV_PUBLISH_USERNAME: ${{ secrets.NEXUS_USERNAME}}
          UV_PUBLISH_PASSWORD: ${{ secrets.NEXUS_PASSWORD}}
        run: |
          uv build --no-sources
          uv publish --index nexus


```

---

_Comment by @zanieb on 2025-02-13 02:29_

It looks like it's failing on `uv build` because you don't have `UV_INDEX_NEXUS_PASSWORD` set for read permissions?

---

_Comment by @shoaibkhanz on 2025-02-13 02:43_

that actually fixed it thanks, I didnt think building would need access to nexus, thought that would happen within CI and then pushed to nexus, which indeed not the case.

---

_Comment by @shoaibkhanz on 2025-02-13 02:45_

I have follow up question though, would I need to then have `UV_INDEX_NEXUS_PASSWORD` and `UV_PUBLISH_PASSWORD` , that seem to be the case

---

_Comment by @shoaibkhanz on 2025-02-13 02:51_

I understand that during build uv isnt only packaging my code, its verifying and resolving the dependency as well and hence needs index username and password.

---

_Comment by @zanieb on 2025-02-13 03:01_

We're building the source distribution (packaging your code) and that requires a build system, in this case, `build-system.requires` is `hatchling` so we need to download it. Presumably you'd be interested in #3957 which would avoid needing to fetch any Python dependencies to build your package.

---

_Closed by @shoaibkhanz on 2025-02-13 11:39_

---
