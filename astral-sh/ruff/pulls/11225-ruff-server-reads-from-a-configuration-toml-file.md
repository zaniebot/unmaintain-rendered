```yaml
number: 11225
title: "`ruff server` reads from a configuration TOML file in the user configuration directory if no local configuration exists"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/user-settings
created_at: 2024-05-01T01:46:06Z
updated_at: 2024-05-01T09:08:51Z
url: https://github.com/astral-sh/ruff/pull/11225
synced_at: 2026-01-10T22:37:02Z
```

# `ruff server` reads from a configuration TOML file in the user configuration directory if no local configuration exists

---

_Pull request opened by @snowsignal on 2024-05-01 01:46_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11158.

A settings file in the ruff user configuration directory will be used as a configuration fallback, if it exists.

## Test Plan

Create a `pyproject.toml` or `ruff.toml` configuration file in the ruff user configuration directory.

* On Linux, that will be `$XDG_CONFIG_HOME/ruff/` or `$HOME/.config`
* On macOS, that will be `$HOME/Library/Application Support`
* On Windows, that will be `{FOLDERID_LocalAppData}`

Then, open a file inside of a workspace with no configuration. The settings in the user configuration file should be used.

---

_Label `server` added by @snowsignal on 2024-05-01 01:46_

---

_Comment by @github-actions[bot] on 2024-05-01 01:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @akthe-at on 2024-05-01 02:40_

> ## Summary
> Fixes #11158.
> 
> A settings file in the ruff user configuration directory will be used as a configuration fallback, if it exists.
> 
> ## Test Plan
> Create a `pyproject.toml` or `ruff.toml` configuration file in the ruff user configuration directory.
> 
> * On Linux, that will be `$XDG_CONFIG_HOME/ruff/` or `$HOME/.config`
> * On macOS, that will be `$HOME/Library/Application Support`
> * On Windows, that will be `{FOLDERID_LocalAppData}`
> 
> Then, open a file inside of a workspace with no configuration. The settings in the user configuration file should be used.

I could be reading this wrong but wouldn't the windows directory listed above end up being something like "%USERPROFILE%\AppData\Local" ? The current global config for ruff if not local is looked for in "${config_dir}/ruff/pyproject.toml" correct? Is it by design/desired to have these be different toml files or are they meant/able to live together in the same config toml file? 

---

_@charliermarsh approved on 2024-05-01 04:51_

---

_Comment by @charliermarsh on 2024-05-01 04:51_

The logic here is the same as elsewhere in Ruff -- it'll look in the same place.

---

_@MichaReiser approved on 2024-05-01 07:01_

I expected more changes. This makes it look extremely easy :laughing: 

---

_Merged by @snowsignal on 2024-05-01 09:08_

---

_Closed by @snowsignal on 2024-05-01 09:08_

---

_Branch deleted on 2024-05-01 09:08_

---
