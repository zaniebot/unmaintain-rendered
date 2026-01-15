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
updated_at: 2026-01-15T10:18:45Z
url: https://github.com/astral-sh/uv/pull/17459
synced_at: 2026-01-15T10:44:26Z
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

_Comment by @ryanking13 on 2026-01-15 05:02_

> Does this mean pyodide only support Unix paths on Windows, and what happens if I run e.g. a Python CLI script through a pyodide interpreter that gets called with a Windows path?

Our python CLI script converts all the Windows path to Unix path (e.g. `C:\\hello\\world` ==> `/hello/world`). We also mount the root `C` drive directory to work in that way.

> I'll just note I'm very wary of changing this just for Pyodide, this is a brittle / sensitive part of uv.

Sure, I understand that this is a very core part of UV and don't want to break any other cases. I would be happy to discuss more to find a better way to deal with this.

> I thought we didn't support Pyodide on Windows?

We didn't, but we (Pyodide, Cloudflare) are working on enabling Pyodide on Windows.






---

_Comment by @konstin on 2026-01-15 08:15_

> Our python CLI script converts all the Windows path to Unix path (e.g. `C:\\hello\\world` ==> `/hello/world`). We also mount the root `C` drive directory to work in that way.

Can you expand on where/how this conversion happens? It seems strange to me that an interpreter on Windows wouldn't accept a Windows path in `sys.path`.

---

_Comment by @ryanking13 on 2026-01-15 08:28_

> Can you expand on where/how this conversion happens?

We expand them [here](https://github.com/pyodide/pyodide/blob/ec22acf6230fa80a4e9b2d5ac9e766fd4ee974f5/src/templates/python_cli_entry.mjs#L60), which is a Node.js script which is executed when Pyodide python CLI entry is called.

> It seems strange to me that an interpreter on Windows wouldn't accept a Windows path in sys.path.

Pyodide runs inside a JavaScript sandbox, and cannot access the filesystem directly, In Node.js, we mount the host filesystem to Pyodide, and the filesystem path is converted to a Linux-style path regardless of the host filesystem.

---

_Comment by @ryanking13 on 2026-01-15 08:28_

I am trying a different approach that converts `sys.path` inside the Pyodide environment (https://github.com/pyodide/pyodide/pull/6059). If this works, the changes in uv would probably not be needed.

---

_Comment by @konstin on 2026-01-15 10:18_

We generally assume that Windows interpreters can use Windows paths, and can e.g. use Windows paths inside a templated Python script.

---
