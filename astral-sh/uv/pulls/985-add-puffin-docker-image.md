```yaml
number: 985
title: Add Puffin Docker image
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/docker
created_at: 2024-01-19T00:22:38Z
updated_at: 2024-01-19T01:21:32Z
url: https://github.com/astral-sh/uv/pull/985
synced_at: 2026-01-10T15:39:03Z
```

# Add Puffin Docker image

---

_Pull request opened by @charliermarsh on 2024-01-19 00:22_

Missing piece for the release.

## Test Plan

Built the image locally:

```shell
❯ docker run 99956098e1f8f04e209dcfc4a0afcee67df1fe8a726c164884e67f035b1a0f42
Usage: puffin [OPTIONS] <COMMAND>

Commands:
  pip    Resolve and install Python packages
  venv   Create a virtual environment
  clean  Clear the cache
  help   Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose                Use verbose output
  -n, --no-cache               Avoid reading from or writing to the cache
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: PUFFIN_CACHE_DIR=]
  -h, --help                   Print help
  -V, --version                Print version
```

---

_@zanieb approved on 2024-01-19 00:32_

---

_Merged by @charliermarsh on 2024-01-19 01:21_

---

_Closed by @charliermarsh on 2024-01-19 01:21_

---

_Branch deleted on 2024-01-19 01:21_

---
