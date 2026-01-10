---
number: 9899
title: "is there an easy way to get project name and its version with uv like the output of `poetry version`?"
type: issue
state: closed
author: HCharlie
labels:
  - duplicate
assignees: []
created_at: 2024-12-14T20:39:48Z
updated_at: 2024-12-15T16:55:27Z
url: https://github.com/astral-sh/uv/issues/9899
synced_at: 2026-01-10T01:24:47Z
---

# is there an easy way to get project name and its version with uv like the output of `poetry version`?

---

_Issue opened by @HCharlie on 2024-12-14 20:39_

is there an easy way to get project name and its version with uv like the output of `poetry version`?

example
```
➜  hello-world-cli git:(main) poetry version
hello-world-cli 0.3.4
➜  hello-world-cli git:(main) poetry version -h

Description:
  Shows the version of the project or bumps it when a valid bump rule is provided.

Usage:
  version [options] [--] [<version>]

Arguments:
  version                    The version number or the rule to update the version.

Options:
  -s, --short                Output the version number only
      --dry-run              Do not update pyproject.toml file
      --next-phase           Increment the phase of the current version
  -h, --help                 Display help for the given command. When no command is given display help for the list command.
  -q, --quiet                Do not output any message.
  -V, --version              Display this application version.
      --ansi                 Force ANSI output.
      --no-ansi              Disable ANSI output.
  -n, --no-interaction       Do not ask any interactive question.
      --no-plugins           Disables plugins.
      --no-cache             Disables Poetry source caches.
  -C, --directory=DIRECTORY  The working directory for the Poetry command (defaults to the current working directory).
  -v|vv|vvv, --verbose       Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.

Help:
  The version command shows the current version of the project or bumps the version of
  the project and writes the new version back to pyproject.toml if a valid
  bump rule is provided.

  The new version should ideally be a valid semver string or a valid bump rule:
  patch, minor, major, prepatch, preminor, premajor, prerelease.

```

---

_Comment by @zanieb on 2024-12-15 16:55_

Not yet, please see https://github.com/astral-sh/uv/issues/6298 which is listed in #9452 

---

_Closed by @zanieb on 2024-12-15 16:55_

---

_Label `duplicate` added by @zanieb on 2024-12-15 16:55_

---
