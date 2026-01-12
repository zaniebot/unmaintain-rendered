```yaml
number: 3254
title: "ignore/types: refactor lisp into scheme and lisp types"
type: pull_request
state: open
author: jgarte
labels: []
assignees: []
base: master
head: master
created_at: 2025-12-31T23:28:29Z
updated_at: 2025-12-31T23:41:16Z
url: https://github.com/BurntSushi/ripgrep/pull/3254
synced_at: 2026-01-12T18:23:15Z
```

# ignore/types: refactor lisp into scheme and lisp types

---

_@jgarte_

This PR refactors and cleans up the `lisp` type and adds a new `scheme` type.

Scheme and Lisp (Common Lisp) should be kept separate conceptually as they are different languages. Additionally, Julia code should not show up when searching for Lisp code. We already have an `elisp` type, so people searching for Emacs Lisp code should use that type for clarity and intent.

Lisp type changes:
- Added *.asd (ASDF system definition files - the standard build system for Common Lisp)
- Removed *.el (use `elisp` type instead)
- Removed *.jl (Julia, not a Lisp in the strict sense and we already have a `julia` type)
- Removed *.sc and *.scm (moved to new `scheme` type)

New Scheme type added:
- *.scm - Standard Scheme source code
- *.sc - Scheme source (alternate, rare)
- *.sch - Scheme source (alternate, rare)
- *.sld - R7RS library definition files
- *.sls - R6RS library source files
- *.sps - R6RS program source files

References:
- ASDF: https://asdf.common-lisp.dev/
- Scheme file extensions registry: https://registry.scheme.org/ (See the __Filename extensions__ section)
- R6RS specification: https://r6rs.org/
- R7RS specification: https://small.r7rs.org/

---
