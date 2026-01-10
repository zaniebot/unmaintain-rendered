```yaml
number: 1199
title: "[panic] \"too many cycle iterations\" panic during semantic analysis of dynamic imports"
type: issue
state: closed
author: floraCodes
labels: []
assignees: []
created_at: 2025-09-18T07:18:59Z
updated_at: 2025-09-18T14:45:29Z
url: https://github.com/astral-sh/ty/issues/1199
synced_at: 2026-01-10T02:06:25Z
```

# [panic] "too many cycle iterations" panic during semantic analysis of dynamic imports

---

_Issue opened by @floraCodes on 2025-09-18 07:18_

## Description
The `ty` type checker panics with a cycle detection error when analyzing a Python file containing dynamic imports and complex type inference patterns.

## Error Message
```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking [av_scanning.py](vscode-file://vscode-app/Applications/Visual%20Studio%20Code%20copy.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html): exported_names(Id(12d52)): execute: too many cycle iterations
```

## Environment
- Platform: macOS aarch64
- Version: 0.0.1-alpha.20 (f41f00af1 2025-09-03)
- Python version: 3.9+

## Code Pattern That Triggers Panic
The panic occurs in a file that contains:
1. Dynamic imports inside try/except blocks (`import pyclamd`)
2. Complex class hierarchies with enums and dataclasses
3. Async methods with multiple return types
4. Type annotations with union types and optionals

## Workaround
Currently excluding the file from `ty` checking in `ty.toml`:
```toml
[src]
exclude = ["app/services/av_scanning.py"]

---

_Comment by @floraCodes on 2025-09-18 07:21_

probs related to #256


---

_Closed by @carljm on 2025-09-18 14:45_

---
