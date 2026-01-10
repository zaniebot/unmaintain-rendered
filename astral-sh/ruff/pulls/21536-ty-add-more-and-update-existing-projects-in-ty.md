```yaml
number: 21536
title: "[ty] Add more and update existing projects in `ty_benchmark`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/more-benchmarks
created_at: 2025-11-20T09:21:01Z
updated_at: 2025-11-25T07:58:36Z
url: https://github.com/astral-sh/ruff/pull/21536
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add more and update existing projects in `ty_benchmark`

---

_Pull request opened by @MichaReiser on 2025-11-20 09:21_

## Summary

This PR adds more projects to `ty_benchmark` and updates existing benchmarks. It also adds `pyrefly` as a benchmark target. I also made some improvements to result rendering and added a check that the command fails if any type checker exits due to an error other than typing errors (requires hyperfine 1.20 or newer).

I don't consider this the final set of projects and I'm happy to add more projects (or remove projects) based on your feedback. Overall, it's fairly tricky to select a set of projects because any project that isn't a library tends to use a mypy-plugin or non-strict type checking options which either results in a lot of diagnostics for type checkers other than the one the project is using, because it would require customizing each type checker's configuration to roughly the same settings. Which I'm not convinced is worth the effort. 

We should be careful about drawing early conclusions from the benchmark, especially when comparing ty and pyrefly, because both type checkers are still missing crucial, but different, typing features, where ty is probably a little further behind (at least up to the beta where we add many of those missing large features).

Closes https://github.com/astral-sh/ty/issues/241

I'm not 100% convinced whether we want the snapshotting mechanism, but it's sort of nice to have some way of measuring if the projects still do what one expects them to.


## Test Plan

```
black
-----

Benchmark 1: ty
  Time (mean ± σ):      57.0 ms ±   2.5 ms    [User: 334.9 ms, System: 39.9 ms]
  Range (min … max):    52.8 ms …  64.1 ms    49 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):     159.5 ms ±   9.4 ms    [User: 567.9 ms, System: 179.3 ms]
  Range (min … max):   145.7 ms … 181.8 ms    17 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):      1.188 s ±  0.027 s    [User: 1.132 s, System: 0.052 s]
  Range (min … max):    1.167 s …  1.261 s    10 runs

Benchmark 4: mypy (warm)
  Time (mean ± σ):     130.3 ms ±   1.0 ms    [User: 97.8 ms, System: 30.7 ms]
  Range (min … max):   129.1 ms … 133.7 ms    22 runs

Benchmark 5: Pyright
  Time (mean ± σ):      1.880 s ±  0.069 s    [User: 20.128 s, System: 0.912 s]
  Range (min … max):    1.808 s …  2.040 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    2.29 ± 0.10 times faster than mypy (warm)
    2.80 ± 0.21 times faster than Pyrefly
   20.85 ± 1.02 times faster than mypy
   33.01 ± 1.88 times faster than Pyright

-------------------------------------------------------------------------------

discord.py
----------

Benchmark 1: ty
  Time (mean ± σ):     203.9 ms ±  12.6 ms    [User: 1232.4 ms, System: 94.1 ms]
  Range (min … max):   196.9 ms … 247.0 ms    14 runs

  Warning: Ignoring non-zero exit code.
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: Pyrefly
  Time (mean ± σ):     297.6 ms ±  19.7 ms    [User: 2368.9 ms, System: 260.9 ms]
  Range (min … max):   262.9 ms … 333.3 ms    11 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):      6.795 s ±  0.254 s    [User: 6.683 s, System: 0.105 s]
  Range (min … max):    6.352 s …  7.053 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 4: mypy (warm)
  Time (mean ± σ):      5.804 s ±  0.285 s    [User: 5.709 s, System: 0.090 s]
  Range (min … max):    5.315 s …  6.121 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 5: Pyright
  Time (mean ± σ):      4.377 s ±  0.074 s    [User: 53.869 s, System: 1.943 s]
  Range (min … max):    4.227 s …  4.459 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    1.46 ± 0.13 times faster than Pyrefly
   21.46 ± 1.37 times faster than Pyright
   28.46 ± 2.24 times faster than mypy (warm)
   33.32 ± 2.40 times faster than mypy

-------------------------------------------------------------------------------

homeassistant
-------------

Benchmark 1: ty
  Time (mean ± σ):      1.932 s ±  0.057 s    [User: 20.511 s, System: 2.858 s]
  Range (min … max):    1.841 s …  2.016 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):      5.377 s ±  0.018 s    [User: 22.429 s, System: 35.348 s]
  Range (min … max):    5.353 s …  5.410 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):     41.547 s ±  0.465 s    [User: 39.661 s, System: 1.780 s]
  Range (min … max):   40.391 s … 42.065 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 4: mypy (warm)
  Time (mean ± σ):      2.849 s ±  0.024 s    [User: 1.771 s, System: 1.072 s]
  Range (min … max):    2.821 s …  2.897 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 5: Pyright
  Time (mean ± σ):     52.909 s ±  2.973 s    [User: 467.397 s, System: 29.649 s]
  Range (min … max):   48.636 s … 57.453 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    1.47 ± 0.05 times faster than mypy (warm)
    2.78 ± 0.08 times faster than Pyrefly
   21.50 ± 0.68 times faster than mypy
   27.38 ± 1.74 times faster than Pyright

-------------------------------------------------------------------------------

isort
-----

Benchmark 1: ty
  Time (mean ± σ):      41.5 ms ±   3.9 ms    [User: 163.1 ms, System: 20.1 ms]
  Range (min … max):    36.8 ms …  52.0 ms    66 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):     233.3 ms ±  22.2 ms    [User: 734.0 ms, System: 131.1 ms]
  Range (min … max):   197.6 ms … 263.6 ms    14 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):     565.9 ms ±   1.5 ms    [User: 533.8 ms, System: 30.0 ms]
  Range (min … max):   563.7 ms … 569.3 ms    10 runs

Benchmark 4: mypy (warm)
  Time (mean ± σ):     111.1 ms ±   1.7 ms    [User: 85.4 ms, System: 24.2 ms]
  Range (min … max):   108.7 ms … 115.3 ms    24 runs

Benchmark 5: Pyright
  Time (mean ± σ):      6.068 s ±  0.108 s    [User: 28.978 s, System: 1.129 s]
  Range (min … max):    5.872 s …  6.296 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    2.68 ± 0.25 times faster than mypy (warm)
    5.62 ± 0.75 times faster than Pyrefly
   13.63 ± 1.27 times faster than mypy
  146.12 ± 13.86 times faster than Pyright

-------------------------------------------------------------------------------

jinja
-----

Benchmark 1: ty
  Time (mean ± σ):     125.6 ms ±  11.1 ms    [User: 336.7 ms, System: 28.6 ms]
  Range (min … max):   111.7 ms … 145.3 ms    24 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):     166.6 ms ±   5.6 ms    [User: 554.6 ms, System: 103.5 ms]
  Range (min … max):   159.0 ms … 177.1 ms    17 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):     683.8 ms ±   5.3 ms    [User: 648.0 ms, System: 33.3 ms]
  Range (min … max):   678.0 ms … 693.6 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 4: mypy (warm)
  Time (mean ± σ):     378.2 ms ±   2.7 ms    [User: 339.1 ms, System: 36.9 ms]
  Range (min … max):   375.5 ms … 383.0 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 5: Pyright
  Time (mean ± σ):      3.394 s ±  0.156 s    [User: 29.392 s, System: 1.257 s]
  Range (min … max):    3.220 s …  3.707 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    1.33 ± 0.13 times faster than Pyrefly
    3.01 ± 0.27 times faster than mypy (warm)
    5.45 ± 0.48 times faster than mypy
   27.02 ± 2.68 times faster than Pyright

-------------------------------------------------------------------------------

pandas
------

Benchmark 1: ty
  Time (mean ± σ):     708.6 ms ± 177.8 ms    [User: 4575.6 ms, System: 289.8 ms]
  Range (min … max):   451.3 ms … 999.0 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):      1.762 s ±  0.024 s    [User: 17.146 s, System: 1.652 s]
  Range (min … max):    1.728 s …  1.803 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):     21.097 s ±  0.099 s    [User: 20.840 s, System: 0.249 s]
  Range (min … max):   20.990 s … 21.284 s    10 runs

Benchmark 4: mypy (warm)
  Time (mean ± σ):     260.8 ms ±   1.9 ms    [User: 163.1 ms, System: 95.6 ms]
  Range (min … max):   258.5 ms … 264.1 ms    11 runs

Benchmark 5: Pyright
  Time (mean ± σ):     21.739 s ±  1.244 s    [User: 178.356 s, System: 6.204 s]
  Range (min … max):   20.221 s … 23.368 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  mypy (warm) ran
    2.72 ± 0.68 times faster than ty
    6.76 ± 0.10 times faster than Pyrefly
   80.90 ± 0.69 times faster than mypy
   83.37 ± 4.81 times faster than Pyright

-------------------------------------------------------------------------------

pandas-stubs
------------

Benchmark 1: ty
  Time (mean ± σ):      77.7 ms ±  20.6 ms    [User: 307.2 ms, System: 58.0 ms]
  Range (min … max):    57.2 ms … 119.6 ms    47 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):     375.5 ms ±  63.1 ms    [User: 1137.2 ms, System: 530.6 ms]
  Range (min … max):   313.7 ms … 482.7 ms    10 runs

Benchmark 3: mypy
  Time (mean ± σ):      6.344 s ±  0.618 s    [User: 6.186 s, System: 0.153 s]
  Range (min … max):    5.514 s …  7.611 s    10 runs

Benchmark 4: mypy (warm)
  Time (mean ± σ):     231.5 ms ±   4.6 ms    [User: 161.1 ms, System: 68.5 ms]
  Range (min … max):   224.1 ms … 238.3 ms    12 runs

Benchmark 5: Pyright
  Time (mean ± σ):      8.211 s ±  0.184 s    [User: 43.375 s, System: 2.357 s]
  Range (min … max):    8.016 s …  8.659 s    10 runs

Summary
  ty ran
    2.98 ± 0.79 times faster than mypy (warm)
    4.83 ± 1.52 times faster than Pyrefly
   81.68 ± 23.08 times faster than mypy
  105.72 ± 28.14 times faster than Pyright

-------------------------------------------------------------------------------

prefect
-------

Benchmark 1: ty
  Time (mean ± σ):     205.5 ms ±  95.2 ms    [User: 775.7 ms, System: 124.1 ms]
  Range (min … max):    88.9 ms … 432.5 ms    28 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):     681.9 ms ±  11.3 ms    [User: 2525.8 ms, System: 1435.2 ms]
  Range (min … max):   660.8 ms … 694.6 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
  Time (mean ± σ):     779.0 ms ±  48.5 ms    [User: 739.7 ms, System: 36.6 ms]
  Range (min … max):   733.3 ms … 849.8 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 4: mypy (warm)
  Time (mean ± σ):     253.1 ms ±   4.9 ms    [User: 209.3 ms, System: 41.4 ms]
  Range (min … max):   249.0 ms … 262.0 ms    11 runs

  Warning: Ignoring non-zero exit code.

Benchmark 5: Pyright
  Time (mean ± σ):     12.305 s ±  0.840 s    [User: 90.406 s, System: 3.771 s]
  Range (min … max):   11.105 s … 13.787 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    1.23 ± 0.57 times faster than mypy (warm)
    3.32 ± 1.54 times faster than Pyrefly
    3.79 ± 1.77 times faster than mypy
   59.86 ± 28.02 times faster than Pyright

-------------------------------------------------------------------------------

pytorch
-------

Benchmark 1: ty
  Time (mean ± σ):      1.883 s ±  0.112 s    [User: 15.373 s, System: 0.967 s]
  Range (min … max):    1.628 s …  2.022 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: Pyrefly
  Time (mean ± σ):      2.881 s ±  0.025 s    [User: 21.954 s, System: 9.919 s]
  Range (min … max):    2.828 s …  2.915 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: mypy
Time (mean ± σ):     30.414 s ±  0.623 s    [User: 29.780 s, System: 0.540 s]
  Range (min … max):   29.751 s … 31.527 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 4: mypy (warm)
  Time (mean ± σ):     28.403 s ±  0.232 s    [User: 27.680 s, System: 0.684 s]
  Range (min … max):   28.012 s … 28.752 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 5: Pyright
  Time (mean ± σ):     18.750 s ±  0.674 s    [User: 203.035 s, System: 12.257 s]
  Range (min … max):   17.315 s … 19.962 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ty ran
    1.53 ± 0.09 times faster than Pyrefly
    9.96 ± 0.69 times faster than Pyright
   15.08 ± 0.90 times faster than mypy (warm)
   16.15 ± 1.01 times faster than mypy
```


---

_Label `internal` added by @MichaReiser on 2025-11-20 09:21_

---

_Label `ty` added by @MichaReiser on 2025-11-20 09:21_

---

_@MichaReiser reviewed on 2025-11-20 09:21_

---

_Review comment by @MichaReiser on `scripts/ty_benchmark/src/benchmark/cases.py`:1 on 2025-11-20 09:21_

No, github doesn't recognize the move :(

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 09:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-11-20 12:21_

---

_Review requested from @carljm by @MichaReiser on 2025-11-20 12:21_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-20 12:21_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-20 12:21_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-20 12:21_

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/__init__.py`:40 on 2025-11-20 17:54_

mutable default arguments are [evil](https://docs.astral.sh/ruff/rules/mutable-argument-default/) -- even if this happens to work fine here, I think it would be better to do:

```suggestion
    def run(self, *, cwd: Path | None = None, env: Mapping[str, str] | None = None) -> None:
        env = env or {}
```

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/pyproject.toml`:13 on 2025-11-20 17:57_

we don't depend on mypy anymore? Is it worth adding a comment that says why pyrefly is the only type checker listed here?

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/projects.py`:102 on 2025-11-20 18:16_

these comments indicate the number of diagnostics? Or the time taken in seconds? I thought the latter at first, and was like, "How can mypy complete type-checking instantly -- that seems wrong 😆"

```suggestion
    # Diagnostic count as of Nov 19th 2025:
    # MyPy: 0
    # Pyright: 40
    # ty: 27
    # Pyrefly: 44
```

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/projects.py`:279 on 2025-11-20 18:17_

```suggestion
            "mypy==1.16.0",  # pytorch pins mypy,
```

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/snapshot.py`:60 on 2025-11-20 18:20_

```suggestion
    def run(self, *, cwd: Path, env: Mapping[str, str] | None = None):
        """Run commands and snapshot their output."""
        env = env or {}
        
        # Create snapshot directory if it doesn't exist.
        self.snapshot_dir.mkdir(parents=True, exist_ok=True)
```

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/snapshot.py`:77 on 2025-11-20 18:21_

should we check the return code here and have it raise an exception if it wasn't 0? We can't do that when we actually invoke the type checkers because they might emit diagnostics and therefore exit with code 1, but it seems reasonable to check the returncode for the `prepare` command

```suggestion
                subprocess.run(
                    command.prepare,
                    shell=True,
                    cwd=cwd,
                    env=env,
                    capture_output=True,
                    check=True,
                )
```

---

_Review comment by @AlexWaygood on `scripts/ty_benchmark/src/benchmark/snapshot.py`:122 on 2025-11-20 18:24_

I'd be inclined to use `termcolor` for this (it's a tiny dependency and very stable). I also don't mind having manual ANSI codes too much, though

---

_@AlexWaygood reviewed on 2025-11-20 18:25_

A few small things from skimming!

---

_@MichaReiser reviewed on 2025-11-20 18:32_

---

_Review comment by @MichaReiser on `scripts/ty_benchmark/src/benchmark/snapshot.py`:122 on 2025-11-20 18:32_

Oh I didn't even notice. That was claude :)

---

_Review requested from @charliermarsh by @MichaReiser on 2025-11-20 18:32_

---

_Review request for @carljm removed by @carljm on 2025-11-20 21:59_

---

_@charliermarsh approved on 2025-11-21 00:38_

---

_Review comment by @sharkdp on `scripts/ty_benchmark/README.md`:23 on 2025-11-24 08:06_

What would be an example of such a regression? A case where one type checker suddenly outputs WAY more/fewer diagnostics?

---

_Review comment by @sharkdp on `scripts/ty_benchmark/README.md`:24 on 2025-11-24 08:07_

Can/should we "pin" the platform to an arbitrary one, i.e. pass the equivalent of ty's `--python-platform=linux` to all type checkers?

---

_Review comment by @sharkdp on `scripts/ty_benchmark/pyproject.toml`:25 on 2025-11-24 08:08_

Mainly curious: what's the purpose of this?

---

_Review comment by @sharkdp on `scripts/ty_benchmark/snapshots/pandas-stubs_ty.txt`:4 on 2025-11-24 08:10_

I'll fix those soon :smile: 

---

_Review comment by @sharkdp on `scripts/ty_benchmark/src/benchmark/__init__.py`:55 on 2025-11-24 08:12_

Minor: I prefer to always use the `--long-form` of options when calling tools from scripts
```suggestion
            "--ignore-failure=1",
```

---

_@sharkdp approved on 2025-11-24 08:32_

Thank you!

---

_@sharkdp reviewed on 2025-11-24 08:40_

---

_Review comment by @sharkdp on `scripts/ty_benchmark/src/benchmark/tool.py`:205 on 2025-11-24 08:40_

Hm, we depend on `pyrefly` in `pyproject.toml`, but then use whichever version we get from the PATH? If we want to pin tool versions, we should use the one from the venv?

---

_@MichaReiser reviewed on 2025-11-24 08:49_

---

_Review comment by @MichaReiser on `scripts/ty_benchmark/src/benchmark/tool.py`:205 on 2025-11-24 08:49_

uv adds `pyrefly` to the path, so we always get it from there.

---

_@MichaReiser reviewed on 2025-11-24 08:50_

---

_Review comment by @MichaReiser on `scripts/ty_benchmark/README.md`:24 on 2025-11-24 08:50_

It's tricky because you know Python. 

Solving for one platform requires installing the dependency for that platform, and that failed for me at least for some of the projects on macos. 

---

_Merged by @MichaReiser on 2025-11-25 07:58_

---

_Closed by @MichaReiser on 2025-11-25 07:58_

---

_Branch deleted on 2025-11-25 07:58_

---
