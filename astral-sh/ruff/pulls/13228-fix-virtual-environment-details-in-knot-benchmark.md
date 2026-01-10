```yaml
number: 13228
title: "Fix virtual environment details in `knot_benchmark`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-bench
created_at: 2024-09-03T11:39:44Z
updated_at: 2024-09-03T13:36:37Z
url: https://github.com/astral-sh/ruff/pull/13228
synced_at: 2026-01-10T21:38:32Z
```

# Fix virtual environment details in `knot_benchmark`

---

_Pull request opened by @AlexWaygood on 2024-09-03 11:39_

Following https://github.com/astral-sh/ruff/pull/13227, pyright is looking for a virtual environment in a directory called '--threads', and no such directory exists. This PR fixes that, which slows pyright down again when checking black from 251.9ms to 1.751s on my machine

---

_Renamed from "Fix `--venv-path` for pyright in `knot_benchmark`" to "Fix `--venvpath` for pyright in `knot_benchmark`" by @AlexWaygood on 2024-09-03 11:41_

---

_Comment by @AlexWaygood on 2024-09-03 11:50_

I think this is a fairer comparison between mypy/pyright/redknot... but I'm still seeing lots of errors regarding missing imports from both mypy and pyright if I run `uv run benchmark --project black -vv` locally.

Mypy:

```
src/blackd/middlewares.py:3:1: error: Cannot find implementation or library stub for module named "aiohttp.web_request"  [import-not-found]
src/blackd/middlewares.py:4:1: error: Cannot find implementation or library stub for module named "aiohttp.web_response"  [import-not-found]
src/black/output.py:11:1: error: Cannot find implementation or library stub for module named "click"  [import-not-found]
src/black/report.py:9:1: error: Cannot find implementation or library stub for module named "click"  [import-not-found]
src/black/cache.py:12:1: error: Cannot find implementation or library stub for module named "platformdirs"  [import-not-found]
src/black/files.py:21:1: error: Cannot find implementation or library stub for module named "packaging.specifiers"  [import-not-found]
src/black/files.py:22:1: error: Cannot find implementation or library stub for module named "packaging.version"  [import-not-found]
src/black/files.py:34:1: error: Cannot find implementation or library stub for module named "tomli"  [import-not-found]
```

Pyright:

```
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:20:6 - warning: Import "mypy_extensions" could not be resolved from source (reportMissingModuleSource)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:21:6 - error: Import "packaging.specifiers" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:22:6 - error: Import "packaging.version" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:23:6 - error: Import "pathspec" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:24:6 - error: Import "pathspec.patterns.gitwildmatch" could not be resolved (reportMissingImports)
  /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp1lb8nym4/src/black/files.py:42:12 - warning: Import "colorama" could not be resolved from source (reportMissingModuleSource)
```

So something else may be going wrong somewhere...

---

_Comment by @MichaReiser on 2024-09-03 11:59_

Thanks. I was just about to revert the commit because I don't see any performance improvements by just using the max number of threads (but e.g. 4 threads is faster on my machine for black).

> So something else may be going wrong somewhere...

This is what I pointed out in the original PR summary and asked for help:

> I'm a bit surprised that I e.g. see an error for Black. I would appreciate it if someone could verify that the diagnostics are reasonable.

https://github.com/astral-sh/ruff/pull/13026

---

_@MichaReiser approved on 2024-09-03 11:59_

---

_Label `red-knot` added by @MichaReiser on 2024-09-03 11:59_

---

_Comment by @AlexWaygood on 2024-09-03 12:01_

> This is what I pointed out in the original PR summary and asked for help:

Yeah, it's odd! I checked the output of the tools at the time and I don't remember seeing anything like this. I remember everything looking similar to what I'd expect at the time. I'm looking into it now.

Adding the `--verbose` flag to pyright's invocation in the script indicates that it is successfully finding the virtual environment:

```
Could not resolve source for 'mypy_extensions' in file '/private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/src/black/nodes.py'
  Looking in root directory of execution environment 'file:///private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92'
  Attempting to resolve using root path 'file:///private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92'
  Looking in extraPath 'file:///private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/src'
  Attempting to resolve using root path 'file:///private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/src'
  Finding python search paths
  Found path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib'; looking for site-packages
  Did not find 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib/site-packages', so looking for python subdirectory
  Found path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib/python3.12/site-packages'
  Did not find 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib64'
  Found path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/Lib'; looking for site-packages
  Did not find 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/Lib/site-packages', so looking for python subdirectory
  Found path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/Lib/python3.12/site-packages'
  Found the following 'site-packages' dirs
    file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib/python3.12/site-packages
  Looking in python search path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib/python3.12/site-packages'
  Attempting to resolve using root path 'file:///var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpesdbtb92/venv/lib/python3.12/site-packages'
```

---

_Comment by @MichaReiser on 2024-09-03 12:04_

Yeah it's not that all packages are missing, just some. The packages that are missing are also missing in the dependencies arrays (which I took from mypy-primer)

---

_Comment by @AlexWaygood on 2024-09-03 12:07_

> The packages that are missing are also missing in the dependencies arrays (which I took from mypy-primer)

In the logs I posted above, I see mypy errors about not being able to resolve `click`, and pyright errors about not being able to resolve `packaging`. Both of those should be `py.typed` packages we have installed into the virtual environment, no?

https://github.com/astral-sh/ruff/blob/c2aac5f8263841725728d82fc643023466f576b8/scripts/knot_benchmark/src/benchmark/projects.py#L96-L109

---

_Comment by @MichaReiser on 2024-09-03 12:12_

This is the only error that I get when running mypy over black

```
Benchmark 1: mypy
src/blackd/__init__.py:91:22: error: List item 0 has incompatible type "Callable[[Request, Callable[[Request], Awaitable[StreamResponse]]], Awaitable[StreamResponse]]"; expected "Middleware"  [list-item]
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Found 1 error in 1 file (checked 40 source files)
```

---

_@MichaReiser approved on 2024-09-03 13:33_

---

_Renamed from "Fix `--venvpath` for pyright in `knot_benchmark`" to "Fix virtual environment details in `knot_benchmark`" by @AlexWaygood on 2024-09-03 13:34_

---

_Comment by @AlexWaygood on 2024-09-03 13:35_

(We debugged this offline -- it seems like something changed in uv between v0.3 and v0.4? We're not sure what, but specifying `--python` for the `uv pip install` fixes things, and is more explicit anyway 😄)

---

_Merged by @AlexWaygood on 2024-09-03 13:35_

---

_Closed by @AlexWaygood on 2024-09-03 13:35_

---

_Branch deleted on 2024-09-03 13:35_

---

_Comment by @codspeed-hq[bot] on 2024-09-03 13:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-bench)

### Merging #13228 will **degrade performances by 4.45%**

<sub>Comparing <code>alex/redknot-bench</code> (61ee689) with <code>main</code> (c2aac5f)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-bench)._

### Benchmarks breakdown

|     | Benchmark | `main` | `alex/redknot-bench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.45% |


---
