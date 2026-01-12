```yaml
number: 13235
title: "Make mypy pass on black in `knot_benchmark`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/black-bench
created_at: 2024-09-03T20:07:17Z
updated_at: 2024-09-04T09:35:59Z
url: https://github.com/astral-sh/ruff/pull/13235
synced_at: 2026-01-12T15:55:43Z
```

# Make mypy pass on black in `knot_benchmark`

---

_@AlexWaygood_

## Summary

This fixes one of the problems highlighted in astral-sh/ty#241 in the `knot_benchmark` script. The issue was that recent versions of `aiohttp` led to typing errors in black. These were fixed in https://github.com/psf/black/commit/699b45aef7c05cbec25019ed3c990f97e8b3c944, but we pinned a commit of black prior to that in `knot_benchmark`; meanwhile, we were installing the latest version of `aiohttp`, because our dependencies for the projects being benchmarked were not pinned.

This PR bumps the pinned commit of black to a newer version, and adds a `--exclude-newer` flag (set to today's date) to the installation command so that we won't be impacted by issues like this in the future.

## Test Plan

```
~/dev/ruff/scripts/knot_benchmark (alex/black-bench)⚡ % uv run benchmark --project=black -vv --mypy --min-runs=1 --benchmark cold
2024-09-03 21:02:05 INFO Cloned black to /var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp35jq5s6j.
black (cold)
2024-09-03 21:02:05 INFO Running ['hyperfine', '-i', '--show-output', '--warmup', '3', '--min-runs', '1', '--command-name', 'mypy', '--prepare', '', '/Users/alexw/dev/ruff/scripts/knot_benchmark/.venv/bin/mypy --python-executable /var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmp35jq5s6j/venv/bin/python src --no-incremental --cache-dir /dev/null']
Benchmark 1: mypy
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Success: no issues found in 40 source files
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Success: no issues found in 40 source files
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Success: no issues found in 40 source files
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Success: no issues found in 40 source files
Warning: unused section(s) in pyproject.toml: module = ['tests.data.*']
Success: no issues found in 40 source files
  Time (mean ± σ):      1.485 s ±  0.012 s    [User: 1.398 s, System: 0.084 s]
  Range (min … max):    1.476 s …  1.493 s    2 runs
```


---

_Label `red-knot` added by @AlexWaygood on 2024-09-03 20:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-03 20:07_

---

_@carljm approved on 2024-09-03 21:26_

---

_@MichaReiser approved on 2024-09-04 06:18_

Thanks

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:211 on 2024-09-04 06:19_

Should we make this a more prominent constant that's easier to find / is defined in `projects`?

---

_@MichaReiser reviewed on 2024-09-04 06:19_

---

_@AlexWaygood reviewed on 2024-09-04 09:17_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:211 on 2024-09-04 09:17_

I think in practice it would only make it harder to discover the value of the constant if we defined it as a global somewhere -- if I wanted to find out the exact command passed when installing dependencies I would grep for `"uv pip"` or `"pip"` in the source code, and I'd find this list of arguments fairly easily. (I know this because it's exactly what I did while debugging https://github.com/astral-sh/ruff/pull/13228 ;) But if this value was defined in another file, I'd then have to add an additional step of going to look in that other file.

I'll add a comment about why we pass the argument, however!

---

_@MichaReiser reviewed on 2024-09-04 09:20_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:211 on 2024-09-04 09:20_

I would find a comment at the top of projects useful just in case someone wonders why the dependencies are all outdated

---

_Merged by @AlexWaygood on 2024-09-04 09:35_

---

_Closed by @AlexWaygood on 2024-09-04 09:35_

---

_Branch deleted on 2024-09-04 09:35_

---
