---
number: 17089
title: uv tool --profile XXX run YYY
type: issue
state: open
author: gsemet
labels:
  - enhancement
assignees: []
created_at: 2025-12-11T18:17:45Z
updated_at: 2025-12-11T18:20:33Z
url: https://github.com/astral-sh/uv/issues/17089
synced_at: 2026-01-10T01:26:13Z
---

# uv tool --profile XXX run YYY

---

_Issue opened by @gsemet on 2025-12-11 18:17_

### Summary

I would like to make the following request for a feature for `uv tool` series of commands:
```bash
$ uv tool --profile XXX run YYY
```

Where:
- `XXX`: a URL, git+http, or local path to descriptor with out without lockfile. This serves as reference of what is the list of tools with version to use for a given environment
- `YYY`: a command line to use

Use case:
- "small tools" used in environment. For example, let's say I have a internal software (say, C, matlab,...) with custom build system, I still would like to execute a few tools with `uv tool run` but with a controlled environment (a central list of tool to use, with version and lockfile). For instance freeze the version of pylint or ruff for fast script checking. The use case is when the project is not a classic python pyproject.toml
- it shall be able to fetch from git this tool lockfile and setup a single virtual env (actually i do not have any requirement on that, not sure if we want that or not, but at least the version of all dependencies of each tool needs to be frozen into a lockfile, either one lockfile for all tools or one lockfile per tool, no preference)
- a lock command shall be provided (`uv tool lock toolname1 'toolname1>2' toolname!=1.2.3' ...` 
- ref shall be supported in the http url to allow changing the branch for instance

### Example

```bash
$ uv tool lock pylint ruff 'shfmt-py@3.12.0.1'
Locking...
Generated: uv-tool.lock
$ git add
$ git commit
$ git push ...
```

Later on another environemnt

```bash
$ uv tool --profile git+http://github.com/path/to/project.git#master install shfmt-py
Installing ....


$ uv tool --profile git+http://github.com/path/to/project.git#master install pylint --disable=XXX --colors src
Pylint ...
```

Ideally, linked to https://github.com/astral-sh/uv/issues/12903, no need to execute any network connectivity once the lockfile as been downloaded and installed

This ticket is a more detailed version of https://github.com/astral-sh/uv/issues/12663. it was very close, and would require to have 1 lock file per tool

---

_Label `enhancement` added by @gsemet on 2025-12-11 18:17_

---
