```yaml
number: 1590
title: No file-level diagnostics for project-external files
type: issue
state: closed
author: sharkdp
labels:
  - server
assignees: []
created_at: 2025-11-19T11:10:06Z
updated_at: 2025-11-19T12:20:35Z
url: https://github.com/astral-sh/ty/issues/1590
synced_at: 2026-01-12T15:54:25Z
```

# No file-level diagnostics for project-external files

---

_@sharkdp_

### Summary

I might be wrong, but I seem to remember that I used to get file-level diagnostics in project-external files. For example: I have the `~/ruff` folder opened in VS Code. If I open a file from `~/playground`, I don't see any diagnostics:

<img width="344" height="116" alt="Image" src="https://github.com/user-attachments/assets/60be60e6-7235-4fbb-bb44-02c0bdb5db73" />

Everything works fine if the file comes from within the project:

<img width="358" height="127" alt="Image" src="https://github.com/user-attachments/assets/bd1869f7-4baf-459e-a0fe-26669c9aeaf3" />

And everything also works fine if I open `~/playground` as a folder.

I'm not sure if something in VS Code has changed, or if something in ty has changed.

Choosing a different `"ty.diagnosticMode"` does not help.

### Version

ffce0de3c 2025-11-19

---

_Label `bug` added by @sharkdp on 2025-11-19 11:10_

---

_Label `server` added by @sharkdp on 2025-11-19 11:10_

---

_Comment by @MichaReiser on 2025-11-19 11:12_

This is sort of intentional. We only provide basic LSP features for non-project files but no errors because those could be standard library files or just a random file you're looking at.

---

_Label `bug` removed by @MichaReiser on 2025-11-19 11:12_

---

_Comment by @sharkdp on 2025-11-19 11:16_

I certainly wouldn't want to see them in workspace diagnostics. But ideally, I'd like to see them on a per-file basis?

---

_Comment by @MichaReiser on 2025-11-19 11:30_

> I certainly wouldn't want to see them in workspace diagnostics. But ideally, I'd like to see them on a per-file basis?

I think it depends and we may want to introduce a configuration option for this. I'd be very annoyed seeing a ton of diagnostics on standard library files because they aren't annotated or redefine variables in a way that isn't allowed by ty (but completely fine in a file that has typing stubs).

This question then expands into: 

* Should on-save fixes run when hitting save?
* How should we handle auto import completions (may already be an issue today)? We don't know the search paths
* ... 

---

_Comment by @sharkdp on 2025-11-19 12:20_

> I'd be very annoyed seeing a ton of diagnostics on standard library files because they aren't annotated or redefine variables in a way that isn't allowed by ty (but completely fine in a file that has typing stubs).

Both of these things should (ideally) happen a lot less with ty than with other type checkers, but I see your point.

I am not so sure anymore if this ever worked(?), but I now realize that other LSPs like rust-analyzer also don't support it, and it seems potentially difficult or even controversial. So let's maybe close this for now.

---

_Closed by @sharkdp on 2025-11-19 12:20_

---
