```yaml
number: 2294
title: "Implement `ruff linter` subcommand"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: linter-subcommand
created_at: 2023-01-28T07:49:40Z
updated_at: 2023-01-30T04:06:23Z
url: https://github.com/astral-sh/ruff/pull/2294
synced_at: 2026-01-12T04:52:00Z
```

# Implement `ruff linter` subcommand

---

_Pull request opened by @not-my-profile on 2023-01-28 07:49_

Followup PR for #2288.

The subcommand lists all supported upstream linters and their prefixes:

    $ ruff linter
       F Pyflakes
     E/W pycodestyle
     C90 mccabe
       I isort
       N pep8-naming
       D pydocstyle
      UP pyupgrade
     YTT flake8-2020
    # etc...

Just like with the `rule` subcommand `--format json` is supported:

    $ ruff linter --format json
    [
      {
        "prefix": "F",
        "name": "Pyflakes"
      },
      {
        "prefix": "",
        "name": "pycodestyle",
        "categories": [
          {
            "prefix": "E",
            "name": "Error"
          },
          {
            "prefix": "W",
            "name": "Warning"
          }
        ]
      },
      # etc...


---

_Marked ready for review by @not-my-profile on 2023-01-28 15:23_

---

_@charliermarsh reviewed on 2023-01-29 22:00_

---

_Review comment by @charliermarsh on `ruff_cli/src/commands/linter.rs`:10 on 2023-01-29 22:00_

I know this is really annoying of me but can we use `linter` in lieu of `l` here, and in general use full names rather than abbreviations?

---

_Review comment by @charliermarsh on `ruff_cli/src/commands/linter.rs`:10 on 2023-01-30 02:25_

I went ahead and changed this, hopefully that's ok!

---

_@charliermarsh reviewed on 2023-01-30 02:25_

---

_Merged by @charliermarsh on 2023-01-30 02:32_

---

_Closed by @charliermarsh on 2023-01-30 02:32_

---

_@not-my-profile reviewed on 2023-01-30 04:06_

---

_Review comment by @not-my-profile on `ruff_cli/src/commands/linter.rs`:10 on 2023-01-30 04:06_

Yes, that's alright :) Thanks!

---
