```yaml
number: 6170
title: "Hide global options in `uv generate-shell-completion`"
type: pull_request
state: merged
author: Di-Is
labels:
  - documentation
  - cli
assignees: []
merged: true
base: main
head: hide-global-option
created_at: 2024-08-17T16:12:37Z
updated_at: 2024-08-18T11:04:25Z
url: https://github.com/astral-sh/uv/pull/6170
synced_at: 2026-01-12T16:07:15Z
```

# Hide global options in `uv generate-shell-completion`

---

_@Di-Is_

Resolve #6152 

## Summary

## Test Plan

Execution result of `cargo run generate-shell-completion --help`

```bash
Generate shell completion

Usage: uv generate-shell-completion <SHELL>

Arguments:
  <SHELL>  The shell to generate the completion script for [possible values: bash, elvish, fish, nushell, powershell, zsh]
```

Execution result of `cargo run help generate-shell-completion`

```bash
Generate shell completion

Usage: uv generate-shell-completion <SHELL>

Arguments:
  <SHELL>
          The shell to generate the completion script for
          
          [possible values: bash, elvish, fish, nushell, powershell, zsh]
```

---

_Marked ready for review by @Di-Is on 2024-08-17 16:29_

---

_@zanieb approved on 2024-08-17 18:34_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-08-17 18:34_

---

_Label `cli` added by @zanieb on 2024-08-17 18:34_

---

_Merged by @zanieb on 2024-08-17 18:34_

---

_Closed by @zanieb on 2024-08-17 18:34_

---

_Renamed from "Hide global option in `uv generate-shell-completion`" to "Hide global options in `uv generate-shell-completion`" by @zanieb on 2024-08-17 18:34_

---

_Branch deleted on 2024-08-18 11:04_

---
