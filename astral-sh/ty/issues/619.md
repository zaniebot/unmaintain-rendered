```yaml
number: 619
title: Track open files in server
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-06-09T18:27:40Z
updated_at: 2025-07-18T14:03:36Z
url: https://github.com/astral-sh/ty/issues/619
synced_at: 2026-01-10T02:07:36Z
```

# Track open files in server

---

_Issue opened by @MichaReiser on 2025-06-09 18:27_

https://github.com/astral-sh/ruff/pull/18482 starts purging AST nodes after checking a file is complete. However, purging the AST is undesired for open files in the server because providing completions or any other AST based information then requires reparsing the file. 

The `Project` already has infrastructure to track the open files but we aren't using it in the LSP today. We should extend the LSP to call `open_file` and `close_file` in its notification handlers. 

CC: @ibraheemdev maybe a follow up task to your PR 

---

_Label `server` added by @MichaReiser on 2025-06-09 18:27_

---

_Added to milestone `Beta` by @MichaReiser on 2025-06-09 18:27_

---

_Comment by @dhruvmanila on 2025-06-10 02:10_

Related, I might have to add a new method (or an enum variant in `ProjectFiles`) to force check all files in case workspace diagnostics is requested.

---

_Comment by @MichaReiser on 2025-06-11 05:27_

> Related, I might have to add a new method (or an enum variant in ProjectFiles) to force check all files in case workspace diagnostics is requested.

I think we have two options here:

* Remove the `check` special casing where it only checks a given set of files if `open_files` is set and only use `check_file` in the LSP if the check mode is: Open files only. 
* Keep the `open_files` handling as is, but add a `CheckMode` to the server

It's not clear to me what the better fit is. I do think we want to keep calling `check_file` in `pull_diagnostics` and only call `check` in workspace diagnostics. If that's the case, then making the `check` call in workspace diagnostics conditional on whether the user opted in to workspace or open file diagnostics seems easy enough and it could simplify the project model (which now has multiple fields around "open files").

One other thing that's unclear to me is if removing the open files handling breaks our WASM/benchmark model. 

---

_Label `help wanted` added by @MichaReiser on 2025-06-24 10:40_

---

_Comment by @dhruvmanila on 2025-07-08 09:18_

> The `Project` already has infrastructure to track the open files but we aren't using it in the LSP today. We should extend the LSP to call `open_file` and `close_file` in its notification handlers.

This doesn't work well with workspace diagnostics because the diagnostic builder avoids adding any diagnostic for a closed file which is checked by the `db.is_file_open`.

---

_Closed by @dhruvmanila on 2025-07-18 14:03_

---
