```yaml
number: 10081
title: "Feature Request: Audit for codetags"
type: issue
state: closed
author: mattgiles
labels: []
assignees: []
created_at: 2024-02-22T15:49:58Z
updated_at: 2024-03-20T20:09:44Z
url: https://github.com/astral-sh/ruff/issues/10081
synced_at: 2026-01-12T15:54:49Z
```

# Feature Request: Audit for codetags

---

_@mattgiles_

This is potentially beyond the perimeter of Ruff's core linting/formatting concerns, but I'm curious if there's any appetite to support a simple workflow/command for auditing/printing/outputting codetag comments (`TODO:`, `HACK:`, etc) from target paths. Either as it's own subcommand, or as an option for `ruff check`.

Codetags are obviously extremely common/idiomatic, but as far as I can tell tooling around codetags is minimal (it seems like [rake](https://github.com/ruby/rake) might support such a workflow with `rake notes`).

Minimal tooling may indicate minimal utility/interest. However, I am increasingly aware of CI build steps in the wild that use roll-your-own scripts to audit/report on codetages in similar ways. And I've been surprised at how useful it can be to just see, per file, all codetagged comments in one place.

Cf. [PEP 350 - Codetags](https://peps.python.org/pep-0350)

---

_Closed by @mattgiles on 2024-03-20 20:09_

---
