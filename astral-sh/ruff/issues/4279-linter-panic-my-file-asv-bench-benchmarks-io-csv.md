```yaml
number: 4279
title: "[Linter panic] My File: asv_bench\\benchmarks\\io\\csv.py"
type: issue
state: closed
author: FishAlchemist
labels: []
assignees: []
created_at: 2023-05-08T11:54:41Z
updated_at: 2023-05-08T12:18:58Z
url: https://github.com/astral-sh/ruff/issues/4279
synced_at: 2026-01-10T11:09:47Z
```

# [Linter panic] My File: asv_bench\benchmarks\io\csv.py

---

_Issue opened by @FishAlchemist on 2023-05-08 11:54_

## Version(ruff --version)
**ruff 0.0.265**
## Command(Powershell)
```powershell
ruff check --select=ALL .\csv.py
```
**If you add --isolated , there will be no problem about Linter panic**
## Terminal Content(include stack trace)
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: Linting panicked csv.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: start.raw <= end.raw', C:\Users\runneradmin\.cargo\git\checkouts\rustpython-80c0a11a8fa8e175\c3147d2\ruff_text_size\src\range.rs:48:9
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: BaseThreadInitThunk
  18: RtlUserThreadStart
```
### Command(Powershell): --isolated
```powershell
ruff check --select=ALL --isolated .\csv.py
```
[full_terminal_content_isolated.txt](https://github.com/charliermarsh/ruff/files/11420780/full_terminal_content_isolated.txt)

### Python Code in-place stack trace
```powershell
ruff check --select=ALL .
```
I don't know what the relevant file contents, so I copy dtypes.py to a separate folder with only it and pyproject.toml in it for testing.
This is what the terminal outputs when the file is placed where it should be

```powershell
warning: Linting panicked asv_bench\benchmarks\io\csv.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: start.raw <= end.raw', C:\Users\runneradmin\.cargo\git\checkouts\rustpython-80c0a11a8fa8e175\c3147d2\ruff_text_size\src\range.rs:48:9
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>
  34: <unknown>
  35: BaseThreadInitThunk
  36: RtlUserThreadStart
  ```
## Python Code
**Please change the file extension from txt to py**
[csv.txt](https://github.com/charliermarsh/ruff/files/11420791/csv.txt)
## pyproject.toml
**Please change the file extension from txt to toml**
[pyproject.txt](https://github.com/charliermarsh/ruff/files/11420797/pyproject.txt)



---

_Comment by @MichaReiser on 2023-05-08 12:18_

Thanks for reporting this issue. I verified that this has been fixed by https://github.com/charliermarsh/ruff/pull/4258 and should be included in our next release.

---

_Closed by @MichaReiser on 2023-05-08 12:18_

---
