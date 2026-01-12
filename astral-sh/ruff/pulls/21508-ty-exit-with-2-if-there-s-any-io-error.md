```yaml
number: 21508
title: "[ty] Exit with `2` if there's any IO error"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: micha/io-error-exit-code
created_at: 2025-11-18T08:41:11Z
updated_at: 2025-11-19T08:39:21Z
url: https://github.com/astral-sh/ruff/pull/21508
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Exit with `2` if there's any IO error

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ty/issues/354

Exit with a code of `2` if there's any IO error (because ty failed to read a file).

## Test Plan

Updated test


---

_Label `cli` added by @MichaReiser on 2025-11-18 08:41_

---

_Review requested from @carljm by @MichaReiser on 2025-11-18 08:41_

---

_Label `ty` added by @MichaReiser on 2025-11-18 08:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-18 08:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-18 08:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 08:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @MichaReiser on 2025-11-18 08:48_

Hmm, the mypy primer error is annoying

```
Pythonwin/pywin/test/_dbgscript.py: error[io] Failed to read file: stream did not contain valid UTF-8
```

The file is encoded in latin-1 and contains Umlaute
https://github.com/mhammond/pywin32/blob/main/Pythonwin/pywin/test/_dbgscript.py

I believe this is the desired behavior. But we now need a way to exclude this file 😟 



---

_@MichaReiser reviewed on 2025-11-18 08:53_

---

_Review comment by @MichaReiser on `.github/mypy-primer-ty.toml`:13 on 2025-11-18 08:53_

I think this won't work when running mypy primer locally unless someone uses the `mypy_primer.sh` script.

The alternative is to move `pywin` to `bad.txt`.

---

_@MichaReiser reviewed on 2025-11-18 08:57_

---

_Review comment by @MichaReiser on `.github/mypy-primer-ty.toml`:13 on 2025-11-18 08:57_

Another alternative is to not use an IO error for non-UTF-8 files. 

---

_@sharkdp reviewed on 2025-11-18 09:00_

---

_Review comment by @sharkdp on `.github/mypy-primer-ty.toml`:15 on 2025-11-18 09:00_

I think I would prefer that we patch those in mypy_primer directly. We could pass a `ty_cmd="{ty} check --exclude=… {paths}` option for the corresponding [projects](https://github.com/hauntsaninja/mypy_primer/blob/master/mypy_primer/projects.py).

This way, it would also work locally, and there would be no cross-project pollution of the settings here.

---

_@AlexWaygood reviewed on 2025-11-18 09:03_

---

_Review comment by @AlexWaygood on `.github/mypy-primer-ty.toml`:15 on 2025-11-18 09:03_

We could also just change the primer setup upstream so that primer ignores an exit code 2 from ty without aborting the whole script. It would be just another diagnostic in the diff (like it is today)

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 09:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @astral-sh-bot[bot] on 2025-11-18 09:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `.github/mypy-primer-ty.toml`:15 on 2025-11-18 09:27_

Done https://github.com/hauntsaninja/mypy_primer/pull/221

---

_@MichaReiser reviewed on 2025-11-18 09:27_

---

_@AlexWaygood approved on 2025-11-18 14:12_

---

_Comment by @sharkdp on 2025-11-18 18:28_

Running ecosystem-analyzer, just to make sure that it also picks up the mypy_primer changes.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-18 18:28_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 18:34_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `pywin32` | success | abnormal exit | `1` | `2` |
| `dd-trace-py` | success | abnormal exit | `1` | `2` |

No diagnostic changes detected ✅
**[Full report with detailed diff](https://micha-io-error-exit-code.ecosystem-663.pages.dev/diff)** ([timing results](https://micha-io-error-exit-code.ecosystem-663.pages.dev/timing))




---

_Comment by @sharkdp on 2025-11-18 18:37_

Ok, it does not automatically pick it up. I need to update the lockfile.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-18 18:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-18 18:57_

---

_Merged by @MichaReiser on 2025-11-19 08:39_

---

_Closed by @MichaReiser on 2025-11-19 08:39_

---

_Branch deleted on 2025-11-19 08:39_

---
