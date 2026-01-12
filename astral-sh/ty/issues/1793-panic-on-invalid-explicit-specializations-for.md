```yaml
number: 1793
title: "Panic on invalid explicit specializations for `Final`/`ClassVar`"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - fatal
assignees: []
created_at: 2025-12-06T21:01:07Z
updated_at: 2025-12-07T14:27:16Z
url: https://github.com/astral-sh/ty/issues/1793
synced_at: 2026-01-12T15:54:25Z
```

# Panic on invalid explicit specializations for `Final`/`ClassVar`

---

_@MeGaGiGaGon_

### Summary

On a hunch after reading #1788 , I tried other special things with that same invalid code and found a new crash with other special things. This code:
```py
import typing
x: typing.Final[(),]  # Also crashes with ClassVar
```
Crashes with the message:
```
error[panic]: Panicked at crates\ty_python_semantic\src\types\infer\builder\annotation_expression.rs:318:34 when checking `C:\Users\GiGaGon\issue.py`: `assertion `left == right` failed
  left: Some(Dynamic(Unknown))
 right: None`
```

<details>
<summary>Running on command line</summary>

```powershell
PS C:\Users\GiGaGon> echo "import typing;x: typing.Final[(),]" > issue.py
PS C:\Users\GiGaGon> uvx ty check issue.py 2>1 | rg assertion
error[panic]: Panicked at crates\ty_python_semantic\src\types\infer\builder\annotation_expression.rs:318:34 when checking `C:\Users\GiGaGon\issue.py`: `assertion `left == right` failed
PS C:\Users\GiGaGon> echo "import typing;x: typing.ClassVar[(),]" > issue.py
PS C:\Users\GiGaGon> uvx ty check issue.py
error[panic]: Panicked at crates\ty_python_semantic\src\types\infer\builder\annotation_expression.rs:318:34 when checking `C:\Users\GiGaGon\issue.py`: `assertion `left == right` failed
  left: Some(Dynamic(Unknown))
 right: None`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Version: 0.0.1-alpha.32 (84a188116 2025-12-05)
info: Args: ["C:\\Users\\GiGaGon\\AppData\\Local\\uv\\cache\\archive-v0\\ct7ovJOSUEWDI2Cd7Zphd\\Scripts\\ty.exe", "check", "issue.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(1401))
             at crates\ty_python_semantic\src\types\infer.rs:103
   1: infer_scope_types(Id(1000))
             at crates\ty_python_semantic\src\types\infer.rs:69
   2: check_file_impl(Id(c00))
             at crates\ty_project\src\lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
PS C:\Users\GiGaGon> uvx ty version
ty 0.0.1-alpha.32 (84a188116 2025-12-05)
```

</details>

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-06 22:00_

---

_Label `fatal` added by @AlexWaygood on 2025-12-06 22:08_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-12-07 13:37_

---

_Closed by @charliermarsh on 2025-12-07 14:27_

---
