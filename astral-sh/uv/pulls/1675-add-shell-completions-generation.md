```yaml
number: 1675
title: Add shell completions generation
type: pull_request
state: merged
author: flyaroundme
labels:
  - cli
assignees: []
merged: true
base: main
head: add-shell-completions
created_at: 2024-02-19T01:31:39Z
updated_at: 2024-02-19T03:43:18Z
url: https://github.com/astral-sh/uv/pull/1675
synced_at: 2026-01-12T16:04:41Z
```

# Add shell completions generation

---

_@flyaroundme_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds cli command / flag (`generate-shell-completion <SHELL>` / `--generate-shell-completion <SHELL>`) to generate the completion script for the given shell. Implemented in exactly the same way as it is done in ruff (https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/lib.rs#L197)

Closes https://github.com/astral-sh/uv/issues/1654

## Test Plan

I've normally tested the generated script manually only for bash shell on Ubuntu 22.04.3
```bash
$ uv --generate-shell-completion bash > /usr/share/bash-completion/completions/uv
$ uv # <TAB>
-q                         -h                         --verbose                  --no-cache                 --version                  clean
-v                         -V                         --no-color                 --cache-dir                pip                        generate-shell-completion
-n                         --quiet                    --color                    --help                     venv                       help
$ uv pip # <TAB>
-q           -n           -V           --verbose    --color      --cache-dir  --version    sync         uninstall    help
-v           -h           --quiet      --no-color   --no-cache   --help       compile      install      freeze
```



---

_Label `cli` added by @zanieb on 2024-02-19 02:20_

---

_@zanieb approved on 2024-02-19 03:42_

Thanks!

---

_Merged by @zanieb on 2024-02-19 03:43_

---

_Closed by @zanieb on 2024-02-19 03:43_

---
