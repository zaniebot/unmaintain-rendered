```yaml
number: 21666
title: Support nested projects within a single LSP session
type: pull_request
state: open
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/lsp-mono-repo
created_at: 2025-11-27T16:57:25Z
updated_at: 2025-12-17T08:07:39Z
url: https://github.com/astral-sh/ruff/pull/21666
synced_at: 2026-01-12T15:57:30Z
```

# Support nested projects within a single LSP session

---

_@MichaReiser_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-11-27 21:31_

---

_Comment by @MichaReiser on 2025-12-02 07:54_

One major concern with this approach is that memory usage is likely to grow significantly (~projects*files) because we parse and analyze the same file multiple time, especially because we start computing auto imports per database

---

_Comment by @Gankra on 2025-12-17 01:04_

I think we'll want to revive this for PEP-723 script support, but only register the members if the file is actually open. This way you'll only get really brutal memory usage if you have a bunch of scripts open *and* they have non-trivial dependencies (e.g. if you have a ton of scripts that import members of a monorepo, transitively importing the entire monorepo).

Notably I think we should skip scripts on workspace diagnostics by default. (...which now that I've said that, that will be annoying if your scripts import monorepo members and you're refactoring your APIs... actually christ how would `rename` even work there oh god).

---

_Comment by @MichaReiser on 2025-12-17 08:07_

> Notably I think we should skip scripts on workspace diagnostics by default. (...which now that I've said that, that will be annoying if your scripts import monorepo members and you're refactoring your APIs... actually christ how would rename even work there oh god).

Yeah, this is a good point. I'm getting more and more convinced that we need a single ty instance for all projects.

---
