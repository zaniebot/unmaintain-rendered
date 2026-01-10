```yaml
number: 16068
title: "Do not search for configuration files in `show_settings()` when `--isolated` is passed"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - needs-decision
assignees: []
base: main
head: show-settings-isolated
created_at: 2025-02-10T00:39:37Z
updated_at: 2025-02-11T16:38:06Z
url: https://github.com/astral-sh/ruff/pull/16068
synced_at: 2026-01-10T19:57:22Z
```

# Do not search for configuration files in `show_settings()` when `--isolated` is passed

---

_Pull request opened by @InSyncWithFoo on 2025-02-10 00:39_

## Summary

Resolves #16056.

`show_settings()` will now take a fast path if `config_arguments.isolated` is `true`, in which case it simply derives `Settings` from `config_arguments` and print that. Unlike the existing branch, it does <em>not</em> print the "resolved" paths.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-10 01:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-02-10 07:50_

I don't think we should special case the command based on the `--isolated` flag. It introduces the risk that the shown settings don't match the *actual* settings which would be kind of bad. I also think that we don't want to change the output based on the `--isolated` flag.

I'd prefer if we can factor out the following logic into its own method:

https://github.com/astral-sh/ruff/blob/b63c2e126b928fab4180eda5d9132552beeee7fe/crates/ruff_workspace/src/resolver.rs#L375-L412

The challenge then is what do with the the paths and I'm not sure. Is there an actual use case for this? I'm otherwise inclined not to fix it. 


---

_Label `needs-decision` added by @MichaReiser on 2025-02-10 07:50_

---

_Comment by @InSyncWithFoo on 2025-02-11 16:38_

Closing as discussed at #16056.

---

_Closed by @InSyncWithFoo on 2025-02-11 16:38_

---
