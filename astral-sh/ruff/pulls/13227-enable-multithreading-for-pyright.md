```yaml
number: 13227
title: Enable multithreading for pyright
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/benchmark-pyright-threads
created_at: 2024-09-03T11:20:20Z
updated_at: 2024-09-03T11:41:50Z
url: https://github.com/astral-sh/ruff/pull/13227
synced_at: 2026-01-12T15:55:43Z
```

# Enable multithreading for pyright

---

_@MichaReiser_

## Summary

Enable multithreading for pyright for a fairer comparison.

```
uv run benchmark  --min-runs=3 --project=black
black (cold)
Benchmark 1: knot
  Time (mean ± σ):      34.4 ms ±   1.7 ms    [User: 126.6 ms, System: 74.6 ms]
  Range (min … max):    31.8 ms …  39.2 ms    75 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: Pyright
  Time (mean ± σ):     245.0 ms ±   4.3 ms    [User: 156.2 ms, System: 32.6 ms]
  Range (min … max):   238.0 ms … 252.4 ms    12 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: mypy
  Time (mean ± σ):      3.270 s ±  0.042 s    [User: 3.128 s, System: 0.135 s]
  Range (min … max):    3.222 s …  3.302 s    3 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  knot ran
    7.11 ± 0.37 times faster than Pyright
   94.97 ± 4.76 times faster than mypy
black (warm)
Benchmark 1: mypy
  Time (mean ± σ):     478.5 ms ±  18.8 ms    [User: 416.1 ms, System: 61.1 ms]
  Range (min … max):   459.0 ms … 507.8 ms    6 runs
 
  Warning: Ignoring non-zero exit code.
```

It improves pyright's performance from ~3s to 245ms



---

_Label `red-knot` added by @MichaReiser on 2024-09-03 11:20_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:137 on 2024-09-03 11:24_

If I run `uv run benchmark` with `-vv` locally on this PR branch, it's clear that pyright is no longer resolving any third-party types installed into the virtual environment, because the `--venvpath` flag requires a value:

```suggestion
            "--threads",
            "--venvpath",
```

---

_@AlexWaygood reviewed on 2024-09-03 11:24_

---

_Merged by @MichaReiser on 2024-09-03 11:24_

---

_Closed by @MichaReiser on 2024-09-03 11:24_

---

_Branch deleted on 2024-09-03 11:24_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:137 on 2024-09-03 11:25_

```
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/cache.py:12:6 - error: Import "platformdirs" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/cache.py:14:6 - error: Import "_black_version" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/cache.py:144:59 - error: Expression of type "dict[str, tuple[float | int | str, ...]]" is incompatible with declared type "Dict[str, Tuple[float, int, str]]" (reportAssignmentType)
/private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/concurrency.py
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/concurrency.py:18:6 - warning: Import "mypy_extensions" could not be resolved from source (reportMissingModuleSource)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/concurrency.py:34:16 - error: Import "uvloop" could not be resolved (reportMissingImports)
/private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:20:6 - warning: Import "mypy_extensions" could not be resolved from source (reportMissingModuleSource)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:21:6 - error: Import "packaging.specifiers" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:22:6 - error: Import "packaging.version" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:23:6 - error: Import "pathspec" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:24:6 - error: Import "pathspec.patterns.gitwildmatch" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpm3ut7zl3/src/black/files.py:42:12 - warning: Import "colorama" could not be resolved from source (reportMissingModuleSource)
```

---

_@AlexWaygood reviewed on 2024-09-03 11:25_

---

_@AlexWaygood reviewed on 2024-09-03 11:41_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:137 on 2024-09-03 11:41_

I filed #13228

---
