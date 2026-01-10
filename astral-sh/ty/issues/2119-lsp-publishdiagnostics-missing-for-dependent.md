```yaml
number: 2119
title: "LSP: PublishDiagnostics missing for dependent files after changes in other files"
type: issue
state: closed
author: ficd0
labels:
  - server
assignees: []
created_at: 2025-12-19T19:29:55Z
updated_at: 2025-12-19T20:06:17Z
url: https://github.com/astral-sh/ty/issues/2119
synced_at: 2026-01-10T01:56:41Z
```

# LSP: PublishDiagnostics missing for dependent files after changes in other files

---

_Issue opened by @ficd0 on 2025-12-19 19:29_

### Summary

_This bug pertains only to the LSP functionality of `ty server`, not any of the type checking._

This bug occurs when using an editor that only supports the "push" model for diagnostics.

**What happens**: With files A and B open, and changing B s.t. in should introduce an error in A: No diagnostic is shown in A _until I make some arbitrary edit_ (e.g. insert a space). After that edit, the diagnostic for A appears.

**Expected**: The server should publish updated diagnostics for A as soon as B changes without needing any edit in A, because the server knows that both A and B are opened by the client.

**Steps to reproduce:**

> [!NOTE]
> This occurs with clients that advertise _only_ the [textDocument/publishDiagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_publishDiagnostics) capability, and cannot _pull_ diagnostics on demand.

1. Open files A and B.
2. Edit B to change something that should affect A (e.g. break an import, API, etc.)
3. Observe that diagnostics in A remain stale.
4. Make a no-op edit in A (add/remove a space). Observe that the expected diagnostic for A suddenly appears.

Suppose I have two files open: A and B. They are both known to `ty` and being watched.

Suppose that A imports some object from B. Then, I change file B such that it introduces an error in A. But when I switch back to A, there is no diagnostic; [textDocument/publishDiagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_publishDiagnostics) has not been run. In order to get `ty` to push the updated diagnostic to file A, I need to make an arbitrary edit to file A, so that my client sends `textDocument/didChange` for A, and that seems to prompt `ty` to publish the diagnostic.

**Question**: does the server only recompute diagnostics for file A when the file itself has changed, or are the diagnostics recomputed when the dependency changes, and they're just not being pushed to the client?

### Version

ty 0.0.4

---

_Label `server` added by @AlexWaygood on 2025-12-19 19:35_

---

_Comment by @MichaReiser on 2025-12-19 19:53_

Thank you. This is tracked in https://github.com/astral-sh/ty/issues/1652

Out of curiosity, what editor are you using?

---

_Closed by @MichaReiser on 2025-12-19 19:53_

---

_Comment by @ficd0 on 2025-12-19 20:06_

Thanks.

> Out of curiosity, what editor are you using?

I'm using [Kakoune](https://kakoune.org/) with the [kakoune-lsp](https://github.com/kakoune-lsp/kakoune-lsp) plugin and the following configuration:

```sh
remove-hooks global lsp-filetype-python
hook -group lsp-filetype-python global BufSetOption filetype=python %{
	set-option buffer lsp_servers %{
		[ty]
		root_globs = ["requirements.txt", "setup.py", "pyproject.toml", ".git", ".hg"]
		args = ["server"]
		settings_section = "ty"
		[ty.settings.ty]
		inlayHints.variableTypes = true
		diagnosticMode = "workspace"
		[ruff]
		root_globs = ["requirements.txt", "setup.py", "pyproject.toml", ".git", ".hg"]
		command = "ruff"
		args = ["server"]

		# leaving commented in case i want to switch back

		# [basedpyright-langserver]
		# root_globs = ["requirements.txt", "setup.py", "pyproject.toml", "pyrightconfig.json", ".git", ".hg"]
		# args = ["--stdio"]
		# settings_section = "basedpyright"
		# [basedpyright-langserver.settings.basedpyright.analysis]
		# typeCheckingMode = "standard"
		# inlayHints.genericTypes = true
	}
}
```

---
