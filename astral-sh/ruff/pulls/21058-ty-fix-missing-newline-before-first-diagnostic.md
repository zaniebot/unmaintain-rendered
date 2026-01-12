```yaml
number: 21058
title: "[ty] Fix missing newline before first diagnostic"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: micha/fix-newline-after-progress
created_at: 2025-10-24T08:21:31Z
updated_at: 2025-10-24T15:35:25Z
url: https://github.com/astral-sh/ruff/pull/21058
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Fix missing newline before first diagnostic

---

_@MichaReiser_

## Summary

Copy pasting ty's output from the CLI into GitHub resulted in a missing newline after `Checked 9/9 files` and the first diagnostic
when they are rendered. This is a known issue in indicatif (https://github.com/console-rs/indicatif/issues/695).

The easiest fix is to clear the progress bar on completion, which I prefer anyways because it reduces the output produced by ty. 

Fixes https://github.com/astral-sh/ty/issues/1424


## Test Plan


```
cargo run --manifest-path ../ruff/Cargo.toml --bin ty -- check 444/
   Compiling ty v0.0.0 (/Users/micha/astral/ruff/crates/ty)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 6.54s
     Running `/Users/micha/astral/ruff/target/debug/ty check 444/`
error[unresolved-import]: Cannot resolve imported module `binary`
 --> 444/lib.py:1:6
  |
1 | from binary import *
  |      ^^^^^^
2 | from py3compat import *
  |
info: Searched in the following paths during module resolution:
info:   1. /Users/micha/astral/test (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /Users/micha/astral/test/.venv/lib/python3.10/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `py3compat`
 --> 444/lib.py:2:6
  |
1 | from binary import *
2 | from py3compat import *
  |      ^^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. /Users/micha/astral/test (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /Users/micha/astral/test/.venv/lib/python3.10/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `construct`
 --> 444/mre.py:3:6
  |
1 | from io import BytesIO
2 |
3 | from construct import Float32l
  |      ^^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. /Users/micha/astral/test (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /Users/micha/astral/test/.venv/lib/python3.10/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 3 diagnostics
```


---

_Review requested from @carljm by @MichaReiser on 2025-10-24 08:21_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-24 08:21_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-24 08:21_

---

_Label `cli` added by @MichaReiser on 2025-10-24 08:21_

---

_Label `ty` added by @MichaReiser on 2025-10-24 08:21_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-10-24 08:21_

---

_Review request for @dcreager removed by @MichaReiser on 2025-10-24 08:21_

---

_Review request for @carljm removed by @MichaReiser on 2025-10-24 08:21_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-10-24 08:21_

---

_Comment by @github-actions[bot] on 2025-10-24 08:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 08:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] Fix missing newline after progress bar" to "[ty] Fix missing newline before first diagnostic" by @MichaReiser on 2025-10-24 08:30_

---

_@ibraheemdev approved on 2025-10-24 15:34_

---

_Merged by @MichaReiser on 2025-10-24 15:35_

---

_Closed by @MichaReiser on 2025-10-24 15:35_

---

_Branch deleted on 2025-10-24 15:35_

---
