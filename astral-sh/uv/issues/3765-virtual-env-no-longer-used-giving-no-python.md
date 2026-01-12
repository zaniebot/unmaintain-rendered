```yaml
number: 3765
title: "VIRTUAL_ENV no longer used, giving \"No Python default installation found in virtual environments\" error"
type: issue
state: closed
author: thundergolfer
labels: []
assignees: []
created_at: 2024-05-22T19:58:54Z
updated_at: 2024-05-23T23:03:08Z
url: https://github.com/astral-sh/uv/issues/3765
synced_at: 2026-01-12T15:58:45Z
```

# VIRTUAL_ENV no longer used, giving "No Python default installation found in virtual environments" error

---

_@thundergolfer_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We use `echo "VIRTUAL_ENV=${Python_ROOT_DIR}" >> $GITHUB_ENV` to avoid needing a virtual env in Github Actions. As of `0.2.0` this has stopped working. 

```
$ pip install uv
Collecting uv
  Downloading uv-0.2.0-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (32 kB)
Downloading uv-0.2.0-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (13.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.0/13.0 MB 185.8 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.2.0

$ uv pip install -U pip setuptools wheel invoke
error: No Python default installation found in virtual environments
Error: Process completed with exit code 2.
```

`python -m uv pip install -U pip setuptools wheel invoke` works so it does seem there's something going on with the Python installation detection. 

* `uv --version` is `0.2.0` (latest)
* Environment is Github Actions Ubuntu 20.04 runner. 

---

_Comment by @joennlae on 2024-05-22 20:00_

I can second that. All my builds that use to work with 0.1.45 fail with this `error: No Python default installation found in virtual environments`.

We do not use the `$GITHUB_ENV` variable it just happens with normal conda envs.

---

_Comment by @zanieb on 2024-05-22 20:01_

Thanks for the report, this sounds like a regression from #3266.

I think pointing to a system installation with `VIRTUAL_ENV` was a hack in the first place though and never officially supported. Why not use `--system` now that we support that?

What's the value of the `${Python_ROOT_DIR}` variable?

---

_Comment by @zanieb on 2024-05-22 20:17_

@joennlae Could you share an example of the `VIRTUAL_ENV` or `CONDA_PREFIX` you are setting?

---

_Comment by @joennlae on 2024-05-22 20:21_

I can just share the `workflow.yml` file:

```yaml
name: Linting

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  checks:
    runs-on: ubuntu-latest
    name: ${{ matrix.quality-command }}
    defaults:
      run:
        shell: bash -el {0}
    strategy:
      matrix:
        quality-command:
          - black
          - ruff
          - mypy
    steps:
      - uses: actions/checkout@v4
      - name: Install environment
        uses: conda-incubator/setup-miniconda@v3
        with:
          environment-file: environment.yml
          activate-environment: 44ai
      - name: Install Dependencies
        run: |
          conda activate 44ai
          pip install uv==0.1.45
          uv pip install -r requirements.txt
          uv pip install -r requirements-dev.txt
```

the `environment.yml`:

```yaml

name: 44ai
channels:
  - defaults
dependencies:
  - python=3.11
  - tesseract
  - ffmpeg
```

I it is not the full environment but this should be reproducible.

---

_Comment by @thundergolfer on 2024-05-22 20:38_

@zanieb 

```
echo $VIRTUAL_ENV
/opt/hostedtoolcache/Python/3.11.9/x64
```

```
ls /opt/hostedtoolcache/Python/3.11.9/x64
Python-3.11.9.tgz  build_output.txt  lib     share
bin                include           python  tools_structure.txt
```

---

_Comment by @thundergolfer on 2024-05-22 20:39_

> Why not use --system now that we support that?

We could!

---

_Comment by @zanieb on 2024-05-22 20:48_

@thundergolfer thanks for the details! Yeah this looks like an unintentional change but I think it's probably for the best if `--system` achieves the intent. I could see us allowing but warning if `VIRTUAL_ENV` points to something that's not actually a virtual environment but I'd rather not complicate it without more justification.

@joennlae looks like you're encountering the same as #3769 which I'm fixing in #3771 — we'll have that fixed asap.

---

_Comment by @thundergolfer on 2024-05-22 21:03_

Yep thanks have made a PR internally to switch to `--system`. Thanks for the quick response 👌.

---

_Closed by @zanieb on 2024-05-22 21:20_

---

_Comment by @matmair on 2024-05-23 17:47_

@zanieb `VIRTUAL_ENV` is still mentioned multiple time in the readme - should probably be removed if it is deprecated/was considered a hack

---

_Comment by @zanieb on 2024-05-23 18:11_

Hey @matmair — you're totally welcome and encouraged to use `VIRTUAL_ENV` still. It just should not be used to point to a standard Python installation that is **not** actually a virtual environment.

---

_Comment by @matmair on 2024-05-23 22:58_

Neither VIRTUAL_ENV nor the --system flag seem to work on GitHub Actions anymore, switching back to pip

---

_Comment by @zanieb on 2024-05-23 23:03_

Please share a reproduction if you can, I'm happy to help.

---
