```yaml
number: 20718
title: "[ty] Add `--venv` as an alias to `--python`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: alex/python-option-aliases
created_at: 2025-10-06T11:21:39Z
updated_at: 2025-10-06T12:03:07Z
url: https://github.com/astral-sh/ruff/pull/20718
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Add `--venv` as an alias to `--python`

---

_@AlexWaygood_

## Summary

This PR adds `--venv` as an alias to `--python` on the ty CLI. The motivation is that if you're just skimming the online docs for the CLI interface, this may make it easier to spot at a glance that this option should be used to specify the location of your virtual environment. (See the changes to the generated `cli.md` file in this PR.)

We shouldn't rename the option to `--venv` because it can also be used to point to a system environment. It's tempting to add a `--venv-path` alias, which would match the name of one of pyright's options, but pyright's `--venv-path` option has different semantics, so that would probably cause more confusion rather than clarifying anything.

Helps with https://github.com/astral-sh/ty/issues/1289

---

_Review requested from @carljm by @AlexWaygood on 2025-10-06 11:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-06 11:21_

---

_Label `cli` added by @AlexWaygood on 2025-10-06 11:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-06 11:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-06 11:21_

---

_Label `ty` added by @AlexWaygood on 2025-10-06 11:21_

---

_Comment by @github-actions[bot] on 2025-10-06 11:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 11:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-10-06 12:01_

I don't love it but sounds reasonable (Do I remember it correctly that this is something you always wanted to do but I was grumpy and said no? 😆 )

---

_Comment by @AlexWaygood on 2025-10-06 12:02_

> (Do I remember it correctly that this is something you always wanted to do but I was grumpy and said no? 😆 )

Hehe, that sounds about right 😆

---

_Merged by @AlexWaygood on 2025-10-06 12:03_

---

_Closed by @AlexWaygood on 2025-10-06 12:03_

---

_Branch deleted on 2025-10-06 12:03_

---
