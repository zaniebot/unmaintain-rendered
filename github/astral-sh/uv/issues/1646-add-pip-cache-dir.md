---
number: 1646
title: "Add `pip cache dir`"
type: issue
state: closed
author: hugovk
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-18T14:07:17Z
updated_at: 2024-02-20T04:29:11Z
url: https://github.com/astral-sh/uv/issues/1646
synced_at: 2026-01-07T13:12:16-06:00
---

# Add `pip cache dir`

---

_Issue opened by @hugovk on 2024-02-18 14:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

# Short version

Add a cross-platform way to find out the uv cache directory (like pip has) for use on GitHub Actions.

# Long version

If we want to cache things installed by `uv pip install` on GitHub Actions, we need to do something like this:

```yaml
- uses: actions/cache@v4
  if: startsWith(runner.os, 'Linux')
  with:
    path: ~/.cache/uv
    key: ${{ runner.os }}-uv-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-uv-

- uses: actions/cache@v4
  if: startsWith(runner.os, 'macOS')
  with:
    path: ~/Library/Caches/uv
    key: ${{ runner.os }}-uv-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-uv-

- uses: actions/cache@v4
  if: startsWith(runner.os, 'Windows')
  with:
    path: ~\AppData\Local\uv\Cache
    key: ${{ runner.os }}-uv-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-uv-
```

Adapted from https://github.com/actions/cache/blob/main/examples.md#multiple-oss-in-a-workflow

Or:

```yaml
jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        include:
        - os: ubuntu-latest
          path: ~/.cache/uv
        - os: macos-latest
          path: ~/Library/Caches/uv
        - os: windows-latest
          path: ~\AppData\Local\uv\Cache
    steps:
    - uses: actions/cache@v4
      with:
        path: ${{ matrix.path }}
        key: ${{ runner.os }}-uv-${{ hashFiles('**/requirements.txt') }}
        restore-keys: |
         ${{ runner.os }}-uv-
```

Adapted from https://github.com/actions/cache/blob/main/examples.md#multiple-oss-in-a-workflow-with-a-matrix

Or we may be able to choose a common directory using the `--cache-dir` flag or `UV_CACHE_DIR` environment variable. 

Originally we had to do the same thing with pip (hence the examples at actions/cache).

But then pip 20.1 added a `pip cache dir` command (https://github.com/pypa/pip/issues/7350 / https://github.com/pypa/pip/pull/8095):

```console
❯ pip cache dir
/Users/hugo/Library/Caches/pip
```

If we add `uv pip cache dir`, we would similarly be able to simplify the config:

```yaml
- name: Get uv cache dir
  id: uv-cache
  run: |
    echo "dir=$(uv cache dir)" >> $GITHUB_OUTPUT

- name: pip cache
  uses: actions/cache@v4
  with:
    path: ${{ steps.pip-cache.outputs.dir }}
    key: ${{ runner.os }}-uv-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-uv-
```

Adapted from https://github.com/actions/cache/blob/main/examples.md#using-pip-to-get-cache-location

---

_Comment by @charliermarsh on 2024-02-18 14:27_

Yeah we should add this, it's trivial.

---

_Label `enhancement` added by @charliermarsh on 2024-02-18 14:27_

---

_Label `compatibility` added by @charliermarsh on 2024-02-18 14:27_

---

_Comment by @charliermarsh on 2024-02-18 15:08_

As part of this, I'll probably move `uv clean` to `uv cache clean`, so we can centralize the cache commands.

---

_Comment by @hugovk on 2024-02-18 15:21_

Out of scope for this issue, could be useful to add some other `pip cache` subcommands:

```console
❯ pip cache --help

Usage:
  pip3 cache dir
  pip3 cache info
  pip3 cache list [<pattern>] [--format=[human, abspath]]
  pip3 cache remove <pattern>
  pip3 cache purge


Description:
  Inspect and manage pip's wheel cache.

  Subcommands:

  - dir: Show the cache directory.
  - info: Show information about the cache.
  - list: List filenames of packages stored in the cache.
  - remove: Remove one or more package from the cache.
  - purge: Remove all items from the cache.

  ``<pattern>`` can be a glob expression or a package name.

Cache Options:
  --format <list_format>      Select the output format among: human (default) or abspath
```

Especially  `pip cache info`, because we will end up with large pip and uv caches on some systems:


```console
❯ pip cache info
Package index page cache location (pip v23.3+): /Users/hugo/Library/Caches/pip/http-v2
Package index page cache location (older pips): /Users/hugo/Library/Caches/pip/http
Package index page cache size: 187.6 MB
Number of HTTP files: 870
Locally built wheels location: /Users/hugo/Library/Caches/pip/wheels
Locally built wheels size: 5.9 MB
Number of locally built wheels: 10
```

Shall I open a new issue for `pip cache info`?


---

_Comment by @charliermarsh on 2024-02-18 15:27_

Yeah feel free to create a separate issue for `pip cache info`. That looks useful. (I don't think we'd attempt to mimic the exact same _output_ (in terms of format, etc.), but we could report the same information.)

---

_Referenced in [astral-sh/uv#1655](../../astral-sh/uv/issues/1655.md) on 2024-02-18 15:45_

---

_Comment by @hugovk on 2024-02-18 15:46_

Thanks, I created https://github.com/astral-sh/uv/issues/1655 and sneaked `pip cache list` in there too.

---

_Referenced in [pybamm-team/PyBaMM#3825](../../pybamm-team/PyBaMM/issues/3825.md) on 2024-02-19 06:07_

---

_Comment by @akx on 2024-02-19 12:12_

Ah, heh, related 😁 https://github.com/actions/setup-python/pull/818

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 03:31_

---

_Referenced in [astral-sh/uv#1734](../../astral-sh/uv/pulls/1734.md) on 2024-02-20 04:24_

---

_Closed by @charliermarsh on 2024-02-20 04:29_

---

_Referenced in [blinklabs-io/dingo#1058](../../blinklabs-io/dingo/pulls/1058.md) on 2025-11-24 23:07_

---
