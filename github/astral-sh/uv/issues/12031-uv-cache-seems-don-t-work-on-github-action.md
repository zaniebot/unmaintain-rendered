---
number: 12031
title: "uv cache seems don't work on github action"
type: issue
state: open
author: shenxiangzhuang
labels:
  - question
assignees: []
created_at: 2025-03-07T04:31:20Z
updated_at: 2025-09-30T14:56:31Z
url: https://github.com/astral-sh/uv/issues/12031
synced_at: 2026-01-07T13:12:18-06:00
---

# uv cache seems don't work on github action

---

_Issue opened by @shenxiangzhuang on 2025-03-07 04:31_

### Question

The github action setting:
```yaml
name: Test

on:
  push:
    branches:
      - master
  pull_request:

jobs:
  ci:
    strategy:
      fail-fast: false
      matrix:
        python-version: ["3.11", "3.12"]
        os: [ubuntu-latest]
    runs-on: ${{ matrix.os }}
    env:
      UV_HTTP_TIMEOUT: 900 # max 15min to install deps
    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        with:
          enable-cache: true
          # Adding cache-dependency-path for better cache invalidation
          cache-dependency-path: |
            pyproject.toml
          cache-key: ${{ runner.os }}-${{ matrix.python-version }}-uv

      - name: Set up Python ${{ matrix.python-version }}
        run: uv python install ${{ matrix.python-version }}

      - name: Install the project
        run: uv sync --all-extras --dev

      - name: Lint with Mypy
        run: uv run mypy .

      - name: Lint with Ruff
        run: uv run ruff check

```

Take [this](https://github.com/ai-glimpse/toyllm/actions/runs/13713720918/job/38354800810) run for example, which still need >10min to install all the deps. So I want to know that is the cache really works with this setting?


### Platform

ubuntu-latest

### Version

latest

---

_Label `question` added by @shenxiangzhuang on 2025-03-07 04:31_

---

_Comment by @charliermarsh on 2025-03-14 00:37_

Have you considered using `uv cache prune --ci`? You're probably caching all the pre-built wheels, which tends not to be that effective. Check out https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-14 00:37_

---

_Referenced in [ai-glimpse/toyllm#103](../../ai-glimpse/toyllm/pulls/103.md) on 2025-03-14 05:14_

---

_Comment by @shenxiangzhuang on 2025-03-14 05:35_

Thanks for your suggestion. I tried it and it seems doesn't speedup the installation process.  According the log from github action, there are many download here:
```
Run uv sync --all-extras --dev
  uv sync --all-extras --dev
  shell: /usr/bin/bash -e {0}
  env:
    UV_HTTP_TIMEOUT: 900
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
Using CPython 3.1[2](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:2).9
Creating virtual environment at: .venv
Resolved 1[3](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:3)5 packages in 0.73ms
   Building toyllm @ file:///home/runner/work/toyllm/toyllm
Downloading babel (9.1MiB)
Downloading tiktoken (1.0MiB)
Downloading torch (760.1MiB)
Downloading nvidia-cudnn-cu12 (63[4](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:4).0MiB)
Downloading nvidia-cublas-cu12 (391.6MiB)
Downloading nvidia-cusparse-cu12 (186.9MiB)
Downloading nvidia-cusolver-cu12 (118.4MiB)
Downloading nvidia-cufft-cu12 (116.0MiB)
Downloading nvidia-curand-cu12 ([5](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:5)3.9MiB)
Downloading nvidia-cuda-nvrtc-cu12 (22.[6](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:6)MiB)
Downloading numpy (17.1MiB)
Downloading nvidia-cuda-cupti-cu12 (13.5MiB)
Downloading mypy (11.9MiB)
Downloading ruff (10.3MiB)
Downloading triton (199.8MiB)
Downloading mkdocs-material (8.3MiB)
Downloading matplotlib ([7](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:8).9MiB)
Downloading sympy (5.9MiB)
Downloading virtualenv (5.7MiB)
Downloading fonttools (4.7MiB)
Downloading pillow (4.3MiB)
Downloading mkdocs (3.7MiB)
Downloading networkx (1.6MiB)
Downloading jedi (1.5MiB)
Downloading nvidia-nccl-cu12 (16[8](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:9).1MiB)
Downloading kiwisolver (1.4MiB)
Downloading setuptools (1.2MiB)
Downloading pygments (1.1MiB)
Downloading nvidia-nvjitlink-cu12 (18.8MiB)
Downloading marimo ([11](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:12).2MiB)
 Downloaded networkx
 Downloaded jedi
 Downloaded kiwisolver
 Downloaded pygments
 Downloaded tiktoken
 Downloaded setuptools
 Downloaded mkdocs
      Built toyllm @ file:///home/runner/work/toyllm/toyllm
 Downloaded virtualenv
 Downloaded pillow
 Downloaded fonttools
 Downloaded sympy
 Downloaded matplotlib
 Downloaded nvidia-cuda-cupti-cu[12](https://github.com/ai-glimpse/toyllm/actions/runs/13850494770/job/38756844562?pr=104#step:5:13)
 Downloaded mkdocs-material
 Downloaded marimo
 Downloaded babel
 Downloaded ruff
 Downloaded mypy
 Downloaded nvidia-nvjitlink-cu12
 Downloaded numpy
 Downloaded nvidia-cuda-nvrtc-cu12
 Downloaded nvidia-curand-cu12
```

---

_Comment by @eifinger on 2025-09-30 14:56_

Hi @shenxiangzhuang.

You can try to [disable cache pruning](https://github.com/astral-sh/setup-uv?tab=readme-ov-file#disable-cache-pruning) to see if your action runs faster. I do however think that you are currently limited by the network I/O of your github runner.

You are also using unsupported inputs `cache-dependency-path` and `cache-key`. I think you are looking for [cache-dependency-glob](https://github.com/astral-sh/setup-uv?tab=readme-ov-file#cache-dependency-glob) and [cache-suffix](https://github.com/astral-sh/setup-uv?tab=readme-ov-file#enable-caching)

---
