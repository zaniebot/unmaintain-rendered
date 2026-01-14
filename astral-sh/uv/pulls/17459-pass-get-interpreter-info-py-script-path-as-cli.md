```yaml
number: 17459
title: "Pass `get_interpreter_info.py` script path as CLI instead of using -c option"
type: pull_request
state: open
author: ryanking13
labels:
  - needs-decision
assignees: []
base: main
head: interpreter-info-run-script
created_at: 2026-01-14T11:38:22Z
updated_at: 2026-01-14T14:12:10Z
url: https://github.com/astral-sh/uv/pull/17459
synced_at: 2026-01-14T14:41:38Z
```

# Pass `get_interpreter_info.py` script path as CLI instead of using -c option

---

_@ryanking13_

## Summary

- Related to Pyodide support (#12729)

We would like to make Pyodide interpreter work in Windows environment.

Currently, interpreter information for the virtualenv is calculated by running the following script:

https://github.com/astral-sh/uv/blob/f3add550afa44c59ea79ae371d4fc49802c44299/crates/uv-python/src/interpreter.rs#L954-L957

The issue with it is that the local `sys.path` directory is not available in the Pyodide environment, especially when running in Windows.

The code above generates the code such as

```py
import sys; sys.path = ["C://Users//uv//cache//path"] + sys.path; from python.get_interpreter_info import main; main()
```

However, the path `C://Users//uv//cache//path` is not available inside the Pyodide (Emscripten) environment, as Emscripten environment uses Unix-style path, mapping the Windows path to Unix path.

Therefore, this PR changes the way the script is invoked. Passing the script directly to the python interpreter so that the path calculation can be handled by the interpreter.

Please let me know if it doesn't seem like a right direction.

## Test Plan

```
cargo run -- venv
cargo run -- venv --python pyodide-3.13.2-emscripten-wasm32-musl
```


---

_Comment by @konstin on 2026-01-14 12:06_

> However, the path `C://Users//uv//cache//path` is not available inside the Pyodide (Emscripten) environment, as Emscripten environment uses Unix-style path, mapping the Windows path to Unix path.

Does this mean pyodide only support Unix paths on Windows, and what happens if I run e.g. a Python CLI script through a pyodide interpreter that gets called with a Windows path?

---

_Comment by @zanieb on 2026-01-14 13:29_

I'll just note I'm very wary of changing this just for Pyodide, this is a brittle / sensitive part of uv.

---

_Comment by @zanieb on 2026-01-14 13:31_

I thought we didn't support Pyodide on Windows?

---

_Label `needs-decision` added by @konstin on 2026-01-14 14:12_

---
