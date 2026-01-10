```yaml
number: 6180
title: "Show generate-shell-completion command in `uv help`"
type: pull_request
state: merged
author: Di-Is
labels:
  - cli
assignees: []
merged: true
base: main
head: show-gen-shell-completion-in-long-help
created_at: 2024-08-18T10:05:55Z
updated_at: 2024-08-18T13:13:29Z
url: https://github.com/astral-sh/uv/pull/6180
synced_at: 2026-01-10T13:09:50Z
```

# Show generate-shell-completion command in `uv help`

---

_Pull request opened by @Di-Is on 2024-08-18 10:05_

Resolve #6151

## Test Plan

Execution result of `cargo run -- help`

```bash
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  run                        Run a command or script (experimental)
  init                       Create a new project (experimental)
  add                        Add dependencies to the project (experimental)
  remove                     Remove dependencies from the project (experimental)
  sync                       Update the project's environment (experimental)
  lock                       Update the project's lockfile (experimental)
  tree                       Display the project's dependency tree (experimental)
  tool                       Run and install commands provided by Python packages (experimental)
  python                     Manage Python versions and installations (experimental)
  pip                        Manage Python packages with a pip-compatible interface
  venv                       Create a virtual environment
  cache                      Manage uv's cache
  version                    Display uv's version
  generate-shell-completion  Generate shell completion
  help                       Display documentation for a command
...
```

Execution result of `cargo run -- -h` and `cargo run -- --help` 

```bash
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  run      Run a command or script (experimental)
  init     Create a new project (experimental)
  add      Add dependencies to the project (experimental)
  remove   Remove dependencies from the project (experimental)
  sync     Update the project's environment (experimental)
  lock     Update the project's lockfile (experimental)
  tree     Display the project's dependency tree (experimental)
  tool     Run and install commands provided by Python packages (experimental)
  python   Manage Python versions and installations (experimental)
  pip      Manage Python packages with a pip-compatible interface
  venv     Create a virtual environment
  cache    Manage uv's cache
  version  Display uv's version
  help     Display documentation for a command
...
```


---

_Marked ready for review by @Di-Is on 2024-08-18 10:49_

---

_@zanieb approved on 2024-08-18 13:13_

Awesome

---

_Label `cli` added by @zanieb on 2024-08-18 13:13_

---

_Merged by @zanieb on 2024-08-18 13:13_

---

_Closed by @zanieb on 2024-08-18 13:13_

---
