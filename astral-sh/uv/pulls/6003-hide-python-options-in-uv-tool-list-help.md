```yaml
number: 6003
title: "Hide python options in `uv tool list` help"
type: pull_request
state: merged
author: eth3lbert
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: tool-list-help
created_at: 2024-08-11T07:55:27Z
updated_at: 2024-08-16T10:14:39Z
url: https://github.com/astral-sh/uv/pull/6003
synced_at: 2026-01-10T13:09:50Z
```

# Hide python options in `uv tool list` help

---

_Pull request opened by @eth3lbert on 2024-08-11 07:55_

## Summary

Closes #5982 .

## Test Plan

```
cargo run tool list --help
```


---

_Renamed from "Hide python options in uv tool list help" to "Hide python options in `uv tool list` help" by @eth3lbert on 2024-08-11 07:55_

---

_Comment by @eth3lbert on 2024-08-11 07:56_

```
List installed tools

Usage: uv tool list [OPTIONS]

Options:
      --show-paths  Whether to display the path to each tool environment and installed executable

Cache options:
  -n, --no-cache               Avoid reading from or writing to the cache, instead using a temporary
                               directory for the duration of the operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]

Global options:
  -q, --quiet                      Do not print any output
  -v, --verbose...                 Use verbose output
      --color <COLOR_CHOICE>       Control colors in output [default: auto] [possible values: auto,
                                   always, never]
      --native-tls                 Whether to load TLS certificates from the platform's native
                                   certificate store [env: UV_NATIVE_TLS=]
      --offline                    Disable network access
      --no-progress                Hide all progress outputs
      --config-file <CONFIG_FILE>  The path to a `uv.toml` file to use for configuration [env:
                                   UV_CONFIG_FILE=]
      --no-config                  Avoid discovering configuration files (`pyproject.toml`,
                                   `uv.toml`) [env: UV_NO_CONFIG=]
  -h, --help                       Display the concise help for this command
  -V, --version                    Display the uv version
```

---

_Comment by @eth3lbert on 2024-08-11 08:00_

Should we raise an "unexpected argument" error when these arguments are passed?

---

_@zanieb reviewed on 2024-08-11 14:46_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2775 on 2024-08-11 14:46_

```suggestion
    // Hide unused global Python options.
```

---

_Comment by @zanieb on 2024-08-11 14:47_

No it needs to be okay to provide them, since they're global options.

---

_Label `cli` added by @zanieb on 2024-08-11 14:47_

---

_Label `preview` added by @zanieb on 2024-08-11 14:47_

---

_@zanieb approved on 2024-08-11 14:47_

---

_Merged by @zanieb on 2024-08-13 16:21_

---

_Closed by @zanieb on 2024-08-13 16:21_

---

_Branch deleted on 2024-08-16 10:14_

---
