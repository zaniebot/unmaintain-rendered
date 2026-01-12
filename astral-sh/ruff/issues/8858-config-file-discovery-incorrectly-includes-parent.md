```yaml
number: 8858
title: Config file discovery incorrectly includes parent config
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2023-11-27T16:33:27Z
updated_at: 2023-11-29T04:21:09Z
url: https://github.com/astral-sh/ruff/issues/8858
synced_at: 2026-01-12T15:54:48Z
```

# Config file discovery incorrectly includes parent config

---

_@bersbersbers_

[The docs](https://docs.astral.sh/ruff/configuration/#config-file-discovery) read:
> "any parent configuration files are ignored"

with a few exceptions, all of which do not apply in the following example (both files have a `[tool.ruff]` section, `--config` is not used, a config file *is* found in the filesystem hierarchy, and nothing is provided on the command line at all).

Still,

```shell
mkdir -p bug1/bug2
echo '[tool.ruff]' > bug1/pyproject.toml
echo 'select = ["ALL"]' >> bug1/pyproject.toml
echo '[tool.ruff]' > bug1/bug2/pyproject.toml
echo 'ignore = ["D203", "D212"]' >> bug1/bug2/pyproject.toml
cd bug1/bug2
ruff .
```

gives

```
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
```

although one of each pair of rules is clearly disabled in `bug1/bug2/pyproject.toml`.

```shell
rm ../pyproject.toml 
ruff .
```
works fine, suggesting that the presence of the parent config does play a role although it should not.

ruff 0.1.5

---

_Comment by @zanieb on 2023-11-27 16:47_

Thanks for the clear reproduction. This is surprising... 

Here's the verbose output:

```
❯ ruff check . --no-cache --verbose
[ruff_cli::resolve][DEBUG] Using configuration file (via parent) at: /repro/bug1/bug2/pyproject.toml
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
[ruff_workspace::resolver][DEBUG] Included path via `include`: "/repro/bug1/bug2/pyproject.toml"
[ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/repro/bug1/bug2/.ruff_cache"
[ruff_cli::commands::check][DEBUG] Identified files to lint in: 13.826167ms
[ruff_cli::diagnostics][DEBUG] Checking: /repro/bug1/bug2/pyproject.toml
[ruff_cli::commands::check][DEBUG] Checked 1 files in: 3.359583ms
```

---

_Label `bug` added by @zanieb on 2023-11-27 16:48_

---

_Assigned to @MichaReiser by @charliermarsh on 2023-11-28 06:23_

---

_Closed by @MichaReiser on 2023-11-29 04:21_

---
