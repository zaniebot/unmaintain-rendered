```yaml
number: 15586
title: "Panic `broken pipe` when trying to calculate number of results in red_knot"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - ty
assignees: []
created_at: 2025-01-19T20:57:32Z
updated_at: 2025-02-25T07:42:54Z
url: https://github.com/astral-sh/ruff/issues/15586
synced_at: 2026-01-10T11:09:57Z
```

# Panic `broken pipe` when trying to calculate number of results in red_knot

---

_Issue opened by @qarmin on 2025-01-19 20:57_

Ruff - 98fccec2e7c20be9a5b99907e912a678a8f29900

Steps to reproduce 
```
git clone https://github.com/pytest-dev/pytest.git
cd pytest
uv venv
source .venv/bin/activate
uv pip install -r pyproject.toml
red_knot --project . --venv-path .venv/ | grep wc -l
```

panic
```
(standard input)

thread 'main' panicked at library/std/src/io/stdio.rs:1123:9:
failed printing to stdout: Broken pipe (os error 32)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `bug` added by @MichaReiser on 2025-01-19 21:45_

---

_Label `red-knot` added by @MichaReiser on 2025-01-19 21:45_

---

_Comment by @MichaReiser on 2025-01-19 21:46_

The, by now, famous Rust `println` bug

---

_Renamed from "Panic `broken pipe` when trying to caluclate number of results in red_knot" to "Panic `broken pipe` when trying to calculate number of results in red_knot" by @AlexWaygood on 2025-01-19 22:06_

---

_Closed by @MichaReiser on 2025-02-25 07:42_

---
