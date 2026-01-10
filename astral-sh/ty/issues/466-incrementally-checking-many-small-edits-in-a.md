```yaml
number: 466
title: Incrementally checking many small edits in a large context
type: issue
state: closed
author: breandan
labels:
  - documentation
  - question
  - server
assignees: []
created_at: 2025-05-20T20:47:32Z
updated_at: 2025-05-20T22:35:14Z
url: https://github.com/astral-sh/ty/issues/466
synced_at: 2026-01-10T02:34:10Z
```

# Incrementally checking many small edits in a large context

---

_Issue opened by @breandan on 2025-05-20 20:47_

### Question

To what extent does `ty` support incremental type checking and is this documented somewhere? I would like the ability to insert a large number of candidate lines (say 10^5 or more) into a local typing context and filter out the ill-typed versions without checking the entire project in full. Do consumers need to go through the LSP and simulate keystrokes or are there endpoints for modifying the AST directly? Thanks.

### Version

_No response_

---

_Label `question` added by @breandan on 2025-05-20 20:47_

---

_Label `documentation` added by @AlexWaygood on 2025-05-20 20:48_

---

_Label `server` added by @AlexWaygood on 2025-05-20 20:48_

---

_Comment by @carljm on 2025-05-20 20:53_

There's certainly no API for modifying the AST directly in memory; the AST is derived from source code.

ty does support incremental checking, either via the LSP server or via the `--watch` CLI flag. If I'm understanding your use case, it sounds like you can run ty with `--watch` and pass just one file to check as CLI argument (you need to ensure that all other files that should be importable are either discoverable in a Python installation you point to with `--python` or in a virtualenv pointed to by `VIRTUAL_ENV`, or are directly importable from the directory where you run ty) and then just repeatedly rewrite that one file with new candidate code you want to check? ty will re-check only the one file, and reuse type information from all other unchanged files without re-computing it.

---

_Comment by @breandan on 2025-05-20 21:31_

I guess that makes sense. Do you plan to add more granular support below the file-level or is this not a high priority?

---

_Comment by @carljm on 2025-05-20 21:45_

We are already much more granular than the file level when it comes to tracking dependencies across multiple files (and lazy evaluation within dependency modules we haven't been asked to check). But when a file itself is edited, we will always fully re-check that file.

I guess this might start to slow down if you wanted to add `10^5` lines to the file-to-check, without removing any of the previous lines (i.e. the file eventually grows to become massive) -- but in this case (if the various lines don't interfere with each other, or even depend on previous ones) then I'm not sure why you even want it to be incremental, just pre-generate a big file with all `10^5` lines and check that file once. If you're doing it incrementally why not just rewrite the file with each version of the code you want to check (removing previous lines), and then it should be very fast.

Incremental parsing or finer-grained incrementality within a modified file is not a high priority, since ty is so fast that in realistic cases it's never a problem to re-check a changed file.

---

_Closed by @breandan on 2025-05-20 22:35_

---
