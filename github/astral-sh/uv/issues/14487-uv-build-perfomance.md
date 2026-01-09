---
number: 14487
title: "`uv_build` perfomance"
type: issue
state: closed
author: chirizxc
labels:
  - question
assignees: []
created_at: 2025-07-07T15:51:31Z
updated_at: 2025-07-07T16:40:39Z
url: https://github.com/astral-sh/uv/issues/14487
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv_build` perfomance

---

_Issue opened by @chirizxc on 2025-07-07 15:51_

### Question

![Image](https://github.com/user-attachments/assets/5d1dfe87-a79c-4e23-a47d-861e6385d36c)

![Image](https://github.com/user-attachments/assets/a761f71f-4276-4bcf-b1fb-60c367004df4)

benchmark was run on the basic `uv init --name <...> --lib --build-backend <...>` template and i'd like to ask about performance, because the gap between flit 1.79 ± 0.35.

### Platform

Windows 10

### Version

uv 0.7.19 (38ee6ec80 2025-07-02)

---

_Label `question` added by @chirizxc on 2025-07-07 15:51_

---

_Comment by @konstin on 2025-07-07 15:52_

What is your question, specifically?

---

_Comment by @chirizxc on 2025-07-07 15:54_

are benchmarks an indication that `uv_build` is not that fast? or should this be measured on larger projects, like `poetry`, which has many folders in the `src`?

---

_Comment by @chirizxc on 2025-07-07 15:55_

although the difference of almost two times seems noticeable

---

_Comment by @konstin on 2025-07-07 16:01_

Can you run the benchmark without Just? You may be measuring overhead from Just. You should also create fresh venv in the hyperfine prepare step, otherwise you're measuring uninstallation too. 

The benchmark numbers in the announcement are from the running benchmarks on Linux and Mac, on Windows the speedup may be less (there's higher filesystem overhead on Windows), though I do expect a more than 2x speedup.

---

_Comment by @chirizxc on 2025-07-07 16:22_

![Image](https://github.com/user-attachments/assets/1c7661a5-6470-4c0b-9f99-e35dc7afa94a)

---

_Comment by @chirizxc on 2025-07-07 16:22_

everything seems to be better without just


---

_Closed by @chirizxc on 2025-07-07 16:23_

---

_Comment by @zanieb on 2025-07-07 16:26_

Also for reference...

```
❯ hyperfine --warmup 3 \
    --prepare 'rm -rf example && uv init example --build-backend uv' -n 'uv' 'uv --project example sync --no-editable' \
    --prepare 'rm -rf example && uv init example --build-backend hatchling' -n 'hatchling' 'uv --project example sync --no-editable' \
    --prepare 'rm -rf example && uv init example --build-backend setuptools' -n 'setuptools' 'uv --project example sync --no-editable' \
    --prepare 'rm -rf example && uv init example --build-backend poetry' -n 'poetry' 'uv --project example sync --no-editable' \
    --prepare 'rm -rf example && uv init example --build-backend pdm' -n 'pdm' 'uv --project example sync --no-editable' \
    --prepare 'rm -rf example && uv init example --build-backend flit' -n 'flit' 'uv --project example sync --no-editable'
Benchmark 1: uv
  Time (mean ± σ):      12.5 ms ±   1.3 ms    [User: 5.3 ms, System: 9.7 ms]
  Range (min … max):    11.2 ms …  19.5 ms    81 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.
 
Benchmark 2: hatchling
  Time (mean ± σ):     160.0 ms ±   7.0 ms    [User: 112.2 ms, System: 46.4 ms]
  Range (min … max):   154.3 ms … 177.7 ms    16 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.
 
Benchmark 3: setuptools
  Time (mean ± σ):     374.0 ms ±   5.9 ms    [User: 261.2 ms, System: 94.6 ms]
  Range (min … max):   363.6 ms … 382.6 ms    10 runs
 
Benchmark 4: poetry
  Time (mean ± σ):     248.7 ms ±   9.6 ms    [User: 172.1 ms, System: 67.8 ms]
  Range (min … max):   237.0 ms … 269.4 ms    10 runs
 
Benchmark 5: pdm
  Time (mean ± σ):     156.6 ms ±   3.6 ms    [User: 117.0 ms, System: 36.5 ms]
  Range (min … max):   152.9 ms … 165.8 ms    15 runs
 
Benchmark 6: flit
  Time (mean ± σ):     102.1 ms ±   2.6 ms    [User: 75.3 ms, System: 26.0 ms]
  Range (min … max):    99.8 ms … 110.8 ms    23 runs
 
Summary
  uv ran
    8.15 ± 0.86 times faster than flit
   12.51 ± 1.31 times faster than pdm
   12.78 ± 1.42 times faster than hatchling
   19.86 ± 2.18 times faster than poetry
   29.86 ± 3.10 times faster than setuptools
```

---
