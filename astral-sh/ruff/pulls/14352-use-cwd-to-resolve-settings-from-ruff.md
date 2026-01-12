```yaml
number: 14352
title: "Use CWD to resolve settings from `ruff.configuration`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/server-user-config
created_at: 2024-11-15T05:12:48Z
updated_at: 2024-11-15T08:15:02Z
url: https://github.com/astral-sh/ruff/pull/14352
synced_at: 2026-01-12T15:55:47Z
```

# Use CWD to resolve settings from `ruff.configuration`

---

_@dhruvmanila_

## Summary

This PR fixes a bug in the Ruff language server where the editor-specified configuration was resolved relative to the configuration directory and not the current working directory.

The existing behavior is confusing given that this config file is specified by the user and is not _discovered_ by Ruff itself. The behavior of resolving this configuration file should be similar to that of the `--config` flag on the command-line which uses the current working directory: https://github.com/astral-sh/ruff/blob/3210f1a23bfe42c5d58c609b602861062eeed2ad/crates/ruff/src/resolve.rs#L34-L48

This creates problems where certain configuration options doesn't work because the paths resolved in that case are relative to the configuration directory and not the current working directory in which the editor is expected to be in. For example, the `lint.per-file-ignores` doesn't work as mentioned in the linked issue along with `exclude`, `extend-exclude`, etc.

fixes: #14282 

## Test Plan

Using the following directory tree structure:
```
.
├── .config
│   └── ruff.toml
└── src
    └── migrations
        └── versions
            └── a.py
```

where, the `ruff.toml` is:
```toml
# 1. Comment this out to test `per-file-ignores`
extend-exclude = ["**/versions/*.py"]

[lint]
select = ["D"]

# 2. Comment this out to test `extend-exclude`
[lint.per-file-ignores]
"**/versions/*.py" = ["D"]

# 3. Comment both `per-file-ignores` and `extend-exclude` to test selection works
```

And, the content of `a.py`:
```py
"""Test"""
```

And, the VS Code settings:
```jsonc
{
  "ruff.nativeServer": "on",
  "ruff.path": ["/Users/dhruv/work/astral/ruff/target/debug/ruff"],
  // For single-file mode where current working directory is `/`
  // "ruff.configuration": "/tmp/ruff-repro/.config/ruff.toml",
  // When a workspace is opened containing this path
  "ruff.configuration": "./.config/ruff.toml",
  "ruff.trace.server": "messages",
  "ruff.logLevel": "trace"
}
```

I also tested out just opening the file in single-file mode where the current working directory is `/` in VS Code. Here, the `ruff.configuration` needs to be updated to use absolute path as shown in the above VS Code settings.

---

_Label `bug` added by @dhruvmanila on 2024-11-15 05:12_

---

_Label `server` added by @dhruvmanila on 2024-11-15 05:12_

---

_Comment by @github-actions[bot] on 2024-11-15 05:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-15 06:39_

---

_@MichaReiser approved on 2024-11-15 06:52_

---

_Merged by @dhruvmanila on 2024-11-15 08:15_

---

_Closed by @dhruvmanila on 2024-11-15 08:15_

---

_Branch deleted on 2024-11-15 08:15_

---
