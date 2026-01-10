```yaml
number: 11065
title: "Automatically install and use a matching uv version to satisfy `requires-version`."
type: issue
state: open
author: mjpieters
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-01-29T14:34:34Z
updated_at: 2025-04-09T20:56:38Z
url: https://github.com/astral-sh/uv/issues/11065
synced_at: 2026-01-10T03:41:47Z
```

# Automatically install and use a matching uv version to satisfy `requires-version`.

---

_Issue opened by @mjpieters on 2025-01-29 14:34_

### Summary

We have some projects running with an older, pinned `uv` version in docker containers, but the lock file is updated by developers with the most recent `uv` version installed. This can lead to problems:

- 0.5.19 added support for omitting dynamic package versions from the lock file. We have some projects with such dynamic versions, but locking with 0.5.19 or newer makes the lock file unreadable by older uv versions.
- If a 0.6.x version was released with explicit backward compatibility conflicts, we'd be further up the creek without a paddle.

We can set `required-version` to prevent using newer uv versions:

```toml
[tool.uv]
required-version = ">=0.5.0,<0.5.19"
```

but then anyone with a newer `uv` can't update the  lockfile as `uv` currently will exit with an error when encountering a `required-version` pin that doesn't match its own version. **UNLESS** they use `uv tool run` to install the older `uv` version to do the work:

```sh
% uv lock
error: Required uv version `>=0.5.0, <0.5.19` does not match the running version `0.5.25`
% uv tool run --with 'uv>=0.5.0,<0.5.19' uv lock
Resolved 149 packages in 994ms
```

Could `uv` do this _directly_ please? Specifically, when the `required-version` requirement can't be met by the current `uv` version, `uv` finds a version of itself that _does_ match the requirement and run the desired command with that version instead.

### Example

When running `uv` with a version that doesn't satisfy the `required-version` requirements for the current project, instead of exiting with an error, `uv` finds a version of itself that does satisfy the requirements:

```sh
% cd "$(mktemp -d)"
% uv init
Initialized project `tmp-luczgh2pmf`
% cat >>pyproject.toml <<EOF
heredoc> [tool.uv]
heredoc> required-version=">=0.5.0,<0.5.19"
heredoc> EOF
% uv sync   # executes as `uv tool run --with "uv$(yq -r '.tool.uv.required-version // ""' pyproject.toml)" uv sync`
Resolved 1 package in 7ms
Audited in 0.02ms
```


---

_Label `enhancement` added by @mjpieters on 2025-01-29 14:34_

---

_Renamed from "Automatically install and use an older uv version to satisfy `requires-version`." to "Automatically install and use a matching uv version to satisfy `requires-version`." by @mjpieters on 2025-01-29 14:35_

---

_Comment by @mjpieters on 2025-01-29 14:46_

Our current work-around is to alias `uv`:

```sh
alias uv='uv tool run --with "uv$(yq -r '\''.tool.uv.required-version // ""'\'' pyproject.toml)" uv'
```

(which requires that you have [`yq` installed](https://mikefarah.gitbook.io/yq#install), and only works if you are currently in the same directory as the `pyproject.toml` file, _and_ won't work if you use `uv.toml` files).

---

_Label `wish` added by @zanieb on 2025-01-29 14:47_

---

_Comment by @zanieb on 2025-01-29 14:47_

This sounds cool, but isn't something I can prioritize right now.

---

_Comment by @juftin on 2025-04-09 20:56_

Adding an example from my duplicate issue:

### Example

You have `uv==0.4.0` on your local machine and the following `pyproject.toml`:
```toml
[project]
name = "example"
version = "1.2.3"
description = "Example Project Description"
readme = "README.md"
requires-python = ">=3.11,<3.12"
dependencies = [
    "apache-airflow==2.8.0"
]

[tool.uv]
required-version = ">=0.6.0,<0.7.0"
```

Rather than raising an error that your `uv` version doesn't comply with the `required-version` when running `uv lock`, `uv` would instead essentially run `uvx "uv>=0.5.0,<0.6.0" lock`

---
