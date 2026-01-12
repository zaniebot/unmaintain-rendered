```yaml
number: 8840
title: Unable to authenticate with Azure artifact feed in github actions
type: issue
state: closed
author: maarten-betman
labels: []
assignees: []
created_at: 2024-11-05T19:33:59Z
updated_at: 2024-11-22T09:08:45Z
url: https://github.com/astral-sh/uv/issues/8840
synced_at: 2026-01-12T15:59:36Z
```

# Unable to authenticate with Azure artifact feed in github actions

---

_@maarten-betman_

Dear Astral team,

First of all thanks for the awesome tools you guys make 😄 

I'm having issue getting authenticated with a private Azure Artifacts feed in GitHub Actions. 

In local development we use artifacts keyring as explained in the [uv docs](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#azure-artifacts). See below snip of `pyproject.toml` which successfully installs the internal dependency.

```toml
[project]
requires-python = ">=3.9"
name = "geo-tools"
version = "0.1.2"
description = "Geotechnical Tools"
readme = "README.md"
dependencies = [
    "pandas<=2.1,>=1.4.3",
    "internal-package>=0.3.0",
]

[tool.uv]
extra-index-url = ["https://VssSessionToken@<org-name>.pkgs.visualstudio.com/<project>/_packaging/<feed>/pypi/simple/"]
keyring-provider = "subprocess"
```

However when running the tests in github actions I get a 401 Unauthorized error from my organizational feed.
I've now set UV_EXTRA_INDEX_URL including a PAT token, but no success unfortunately. See below snip from the github actions file Used several flags after the `uv sync` command:
`--no-cache`
`--index-strategy unsafe-first-match` and `unsafe-best-match`

```yaml
name: Test package on PR

on:
  pull_request:
    branches:
      - dev
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    env:
      UV_CACHE_DIR: ${{ github.workspace }}/.uv-cache
      RUST_LOG: uv=trace
      UV_EXTRA_INDEX_URL: "https://token:${{ secrets.AZURE_DEVOPS_PAT }}@<org>.pkgs.visualstudio.com/<project>/_packaging/<feed>/pypi/simple/"
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.9", "3.10", "3.11" ]

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v3
        with:
          enable-cache: true

      - name: Setup python
        run: uv python install ${{ matrix.python-version }}

      - name: Install dependencies
        run: uv sync --keyring-provider disabled
```
RUST_LOG uv=trace:
[github-actions-log.log](https://github.com/user-attachments/files/17637561/github-actions-log.log)

understand it's hard to debug given you don't have access to the feed, but hope the log clarifies a bit where things are going wrong for me 🤞🏻 

---

_Comment by @zanieb on 2024-11-05 20:22_

Does the username need to be `VssSessionToken` instead of `token`?

Have you considered switching to our [new index settings](https://docs.astral.sh/uv/configuration/indexes/) and using `UV_INDEX_<NAME>_PASSWORD` instead?

---

_Comment by @maarten-betman on 2024-11-05 20:27_

Haven't tried the new index settings yet, will try tomorrow. 
`VssSessionToken` should be present in the url when using the `artifacts-keyring` auth provider. This auth mechanism can't be used in github actions as far as I'm aware.

---

_Comment by @maarten-betman on 2024-11-12 13:09_

Switched to new index setting. Works fine locally, but when running `uv sync` in GH Actions I still get the 401.

When changing the wheel URL in the lock file to include the username and password I can successfully install the dependencies...
The URL with the wheel file location is shown in the 401 Unauthorized error. It's quite different to the registry URL. Could it be that `uv` doesn't add the credentials in the request to the URL that points to the wheel? 

```toml
[[package]]
name = "mpl-templates"
version = "0.3.0"
source = { registry = "https://<org>.pkgs.visualstudio.com/<project>/_packaging/<feed>/pypi/simple/" }
dependencies = [
    { name = "matplotlib" },
    { name = "papersize" },
    { name = "pydantic" },
]
wheels = [
    { url = "https://token:password@<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl", hash = "sha256:...." },
]
```  

---

_Comment by @maarten-betman on 2024-11-13 10:59_

Tried deleting the lock files before `uv sync`. Noticed that uv does use the credentials when getting the metadata but an unauthenticated request to get the wheel;

```log
TRACE Updating cached credentials for https://<org>.pkgs.visualstudio.com/<project>/_packaging/<feed>/pypi/simple/mpl-templates/ to Credentials { username: Username(Some("token")), password: Some("<token>") }
TRACE cached request https://<org>.pkgs.visualstudio.com/<project>/_packaging/<feed>/pypi/simple/mpl-templates/ is not storable because its response has a 'no-store' cache-control directive
TRACE Received package metadata for: mpl-templates
TRACE Selecting candidate for mpl-templates with range >=0.3.0 with 3 remote versions
DEBUG Searching for a compatible version of mpl-templates (>=0.3.0)
TRACE Selecting candidate for mpl-templates with range >=0.3.0 with 3 remote versions
TRACE found candidate for package PackageName("mpl-templates") with range Ranges { segments: [(Included("0.3.0"), Unbounded)] } after 1 steps: "0.3.0" version
TRACE found candidate for package PackageName("mpl-templates") with range Ranges { segments: [(Included("0.3.0"), Unbounded)] } after 1 steps: "0.3.0" version
DEBUG Selecting: mpl-templates==0.3.0 [compatible] (mpl_templates-0.3.0-py3-none-any.whl)
TRACE No cache entry exists for /tmp/.tmpUHbrIk/wheels-v3/index/0c5a42c4bb4da8fc/mpl-templates/mpl_templates-0.3.0-py3-none-any.msgpack
DEBUG No cache entry for: https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=...
TRACE Sending fresh HEAD request for https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=...
TRACE Handling request for https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=...
TRACE Request for https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=90d9891c7dff91387dbeb0bdf5763a4e4cfe532f2c6a7f5e0303569009c61019 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=...
TRACE Attempting unauthenticated request for https://<org>.pkgs.visualstudio.com/163c4536-859e-4e7a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl#sha256=...
```

---

_Comment by @maarten-betman on 2024-11-13 12:46_

So removed the step that removes `uv.lock` and added `--show-settings` to `uv sync` and now it seems to be able to install packages from the private package feed... But now I have a PAT token in the logs of my pipelines. Are there any logs I can output that would be useful @zanieb? 

---

_Comment by @EdAyers on 2024-11-18 14:10_

With the new `UV_INDEX_<NAME>_PASSWORD`, is there a way of getting uv to figure out it needs to get this token from the keyring as described [here](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#using-keyring). as far as I can tell using a keyring token in this way is only supported with UV_INDEX_URL.

---

_Comment by @charliermarsh on 2024-11-18 14:13_

I believe you need to set `UV_INDEX_<NAME>_USERNAME` (but not password -- keyring requires a username) then add `--keyring-provider subprocess`.

---

_Comment by @EdAyers on 2024-11-18 14:14_

Cheers, got it working. Just to be clear: for azure devops: you set `UV_INDEX_<NAME>_USERNAME=VssSessionToken` and set `UV_KEYRING_PROVIDER=subprocess`

---

_Comment by @maarten-betman on 2024-11-18 14:56_

> I believe you need to set `UV_INDEX_<NAME>_USERNAME` (but not password -- keyring requires a username) then add `--keyring-provider subprocess`.

Does this also apply in GitHub actions? Reason for opening the issue is that using keyring in GHA doesn't work.

---

_Comment by @charliermarsh on 2024-11-18 14:58_

Which part? But yes, it should all apply to GHA. You always need a username for keyring, which is a keyring requirement IIUC.

---

_Comment by @maarten-betman on 2024-11-18 15:36_

This is what I get now with:
`UV_INDEX_BOKALYTICS_USERNAME=VssSessionToken` & `UV_KEYRING_PROVIDER=subprocess`

```sh
Run uv sync
  uv sync
  shell: /usr/bin/bash -e {0}
  env:
    UV_PYTHON: 3.1[2](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:2)
    UV_INDEX_<NAME>_PASSWORD: <PAT>
    UV_INDEX_<NAME>_USERNAME: VssSessionToken
    UV_KEYRING_PROVIDER: subprocess
Using CPython [3](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:3).12.7
Creating virtual environment at: .venv
Resolved 67 packages in 0.95ms
  × Failed to download `mpl-templates==0.3.0`
  ├─▶ Failed to fetch:
  │   `https://<org>.pkgs.visualstudio.com/163c[4](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:4)536-8[5](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:5)9e-4e7a-9407-154e3f2313d5/_packaging/2dc[6](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:6)adee-8c7f-478b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url
      (https://<org>.pkgs.visualstudio.com/163c4536-859e-4e[7](https://github.com/Company-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:7)a-9407-154e3f2313d5/_packaging/2dc6adee-8c7f-47[8](https://github.com/Boskalis-Community/geo-tools/actions/runs/11895622841/job/33145635059#step:5:8)b-8b17-d3ac8102c6bb/pypi/download/mpl-templates/0.3/mpl_templates-0.3.0-py3-none-any.whl)
  help: `mpl-templates` was included because `geo-tools==0.1.2`
        depends on `mpl-templates`
Error: Process completed with exit code 1.
```

Also without setting the password to a PAT I get the 401. I've tried most combinations of settings. Method described 5 days ago seemed to work. Would some logs be helpful? 

---

_Comment by @maarten-betman on 2024-11-19 12:52_

of all combination currently the below setup works. This includes adding `--show-settings`. 
Removing it results in the 401.

Looking at the logs, it also seems that dependencies are only installed in the last step and not in the `uv sync` step.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    env:
      RUST_LOG: uv=trace
    strategy:
      fail-fast: false
      matrix:
        python-version: [ "3.11", "3.12", "3.13"]

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        run: curl -LsSf https://astral.sh/uv/install.sh | sh

      - name: Setup python
        run: uv python install ${{ matrix.python-version }}

      - name: Install dependencies
        run: uv sync --show-settings
        env:
          UV_PYTHON: ${{ matrix.python-version }}
          UV_INDEX_<NAME>_PASSWORD: ${{ secrets.AZURE_DEVOPS_PAT }}
          UV_INDEX_<NAME>_USERNAME: "VssSessionToken"

      - name: Run tests
        run: uv run pytest tests/
```

---

_Comment by @maarten-betman on 2024-11-22 09:08_

Found out that the issue was caused by a incorrectly set token at organizational level
Sorry for wasting your time 🙏🏻  

---

_Closed by @maarten-betman on 2024-11-22 09:08_

---
