---
number: 8817
title: "`ruff --config` not listed in `ruff --help"
type: issue
state: closed
author: Ttibsi
labels:
  - question
assignees: []
created_at: 2023-11-22T09:40:50Z
updated_at: 2023-11-22T13:48:17Z
url: https://github.com/astral-sh/ruff/issues/8817
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff --config` not listed in `ruff --help

---

_Issue opened by @Ttibsi on 2023-11-22 09:40_

```sh
❯ ruff --version
ruff 0.1.6
❯ ruff --help
Ruff: An extremely fast Python linter.

Usage: ruff [OPTIONS] <COMMAND>

Commands:
  check    Run Ruff on the given files or directories (default)
  rule     Explain a rule (or all rules)
  config   List or describe the available configuration options
  linter   List all supported upstream linters
  clean    Clear any caches in the current directory and any subdirectories
  format   Run the Ruff formatter on the given files or directories
  version  Display Ruff's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)

For help with a specific command, see: `ruff help <command>`.
 ❯ ruff --config
error: a value is required for '--config <CONFIG>' but none was supplied

For more information, try '--help'.
```

As per the title, you can see that `ruff --config` has no input in `ruff --help`, making it harder to find this exists. 

---

_Comment by @charliermarsh on 2023-11-22 12:13_

You might be looking for `ruff config`, the subcommand, rather than `ruff --config`?

`ruff --config` shows this message because, for backwards compatibility, we treat `ruff` without a subcommand as equivalent to `ruff check` -- so, above, it's as if you ran `ruff check --config`.

---

_Label `question` added by @charliermarsh on 2023-11-22 12:13_

---

_Closed by @charliermarsh on 2023-11-22 13:48_

---
