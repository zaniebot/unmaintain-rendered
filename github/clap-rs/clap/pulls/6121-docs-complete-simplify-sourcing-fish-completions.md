---
number: 6121
title: "docs(complete): Simplify sourcing fish completions"
type: pull_request
state: merged
author: AudaciousAxiom
labels: []
assignees: []
merged: true
base: master
head: docs/complete-simplify-fish-source
created_at: 2025-09-06T11:04:52Z
updated_at: 2025-09-08T14:38:21Z
url: https://github.com/clap-rs/clap/pull/6121
synced_at: 2026-01-07T13:12:20-06:00
---

# docs(complete): Simplify sourcing fish completions

---

_Pull request opened by @AudaciousAxiom on 2025-09-06 11:04_

Unlike most other shells, fish [supports sourcing directly from the standard input](https://github.com/fish-shell/fish-shell/blob/898cc3242b15efc9be514538ff97048ed40c2d77/doc_src/cmds/source.rst), which is simpler and more idiomatic.

This is for instance how jj now recommend sourcing their dynamic completions since https://github.com/jj-vcs/jj/pull/4847.

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->


---
