```yaml
number: 22217
title: "[ty] Add option to disable syntax errors in the language server"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: disable-syntax-errors
created_at: 2025-12-26T23:49:19Z
updated_at: 2025-12-29T14:29:50Z
url: https://github.com/astral-sh/ruff/pull/22217
synced_at: 2026-01-12T15:57:44Z
```

# [ty] Add option to disable syntax errors in the language server

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/2106

Added show_syntax_errors as a global option.

## Test Plan

Added 3 tests, one for notebooks, one for publish diagnostics and one for pull diagnostics.

I think these test each of the cases where we filter the diagnostics based on the syntax error setting.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-26 23:49_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-26 23:49_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-26 23:49_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-26 23:49_

---

_Renamed from "Disable syntax errors" to "[ty] Add option to disable syntax errors in the language server" by @MatthewMckee4 on 2025-12-26 23:49_

---

_Review request for @carljm removed by @carljm on 2025-12-26 23:57_

---

_Label `server` added by @ntBre on 2025-12-27 02:26_

---

_Label `ty` added by @ntBre on 2025-12-27 02:26_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:241 on 2025-12-27 17:07_

I think I would pass `global_settings` here to make it consistent with `to_lsp_diagnostics`

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:78 on 2025-12-27 17:09_

Let's move the check to `to_lsp_diagnostic`. That then also reveals that we need to add similar treatment to the workspace diagnostics.

---

_@MichaReiser reviewed on 2025-12-27 17:10_

Thank you. 

Once this lands, we'll need to add the setting on the ty-vscode side.

---

_@MatthewMckee4 reviewed on 2025-12-27 17:19_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/server/api/diagnostics.rs`:241 on 2025-12-27 17:19_

Wait, is there any reason for us to check for syntax errors here, since we are just checking settings errors, so we shouldn't get any python syntax errors here. 

---

_@MichaReiser reviewed on 2025-12-27 17:21_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:241 on 2025-12-27 17:21_

Lol, probably not. I assumed this is `publish_diagnostics`. So, we can just remove the argument?

---

_@MatthewMckee4 reviewed on 2025-12-27 17:22_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/server/api/diagnostics.rs`:241 on 2025-12-27 17:22_

Thats my bad, will update

---

_@MatthewMckee4 reviewed on 2025-12-27 17:47_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/server/api/diagnostics.rs`:252 on 2025-12-27 17:47_

This clone is a but unfortunate

---

_Merged by @MichaReiser on 2025-12-27 20:24_

---

_Closed by @MichaReiser on 2025-12-27 20:24_

---

_Comment by @jamilraichouni on 2025-12-28 04:30_

Will this be documented here?

https://github.com/astral-sh/ty/blob/main/docs/reference/editor-settings.md

---

_Comment by @MatthewMckee4 on 2025-12-28 05:40_

Yeah, I can do it tomorrow

---

_Branch deleted on 2025-12-29 14:29_

---
