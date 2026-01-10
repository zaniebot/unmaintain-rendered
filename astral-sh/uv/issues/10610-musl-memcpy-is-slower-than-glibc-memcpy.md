---
number: 10610
title: musl memcpy is slower than glibc memcpy
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2025-01-14T18:42:59Z
updated_at: 2025-01-14T18:42:59Z
url: https://github.com/astral-sh/uv/issues/10610
synced_at: 2026-01-10T01:24:55Z
---

# musl memcpy is slower than glibc memcpy

---

_Issue opened by @konstin on 2025-01-14 18:42_

Currently, the uv musl builds ("musllinux") are 10% slower than the uv glibc builds ("manylinux"). This seems to be mainly due to memcpy.

```
$ cargo build --profile profiling && cp target/profiling/uv uv-main
$ cargo build --target x86_64-unknown-linux-musl --profile profiling && cp target/x86_64-unknown-linux-musl/profiling/uv uv-musl
$ hyperfine --warmup 2 "./uv-main pip compile --no-progress scripts/requirements/airflow.in" "./uv-musl pip compile --no-progress scripts/requirements/airflow.in"
Benchmark 1: ./uv-main pip compile --no-progress scripts/requirements/airflow.in
  Time (mean ± σ):     104.3 ms ±   4.4 ms    [User: 118.1 ms, System: 101.4 ms]
  Range (min … max):    99.0 ms … 118.8 ms    28 runs
 
Benchmark 2: ./uv-musl pip compile --no-progress scripts/requirements/airflow.in
  Time (mean ± σ):     116.3 ms ±   3.1 ms    [User: 133.8 ms, System: 100.0 ms]
  Range (min … max):   112.5 ms … 126.8 ms    25 runs
 
Summary
  ./uv-main pip compile --no-progress scripts/requirements/airflow.in ran
    1.11 ± 0.06 times faster than ./uv-musl pip compile --no-progress scripts/requirements/airflow.in
```

glibc:

![Image](https://github.com/user-attachments/assets/e85c0922-e036-4aa8-a790-9c6f7e1c864b)

musl:

![Image](https://github.com/user-attachments/assets/71ca296d-43b9-457e-b68d-464fb0be7592)

memcpy is used for all sorts of containers and operations (musl):

![Image](https://github.com/user-attachments/assets/43e3842b-2cf3-41c5-9004-f123a22cc20a)

Are there any ways to bring the musl performance to parity with the glibc performance?

---

_Label `performance` added by @konstin on 2025-01-14 18:42_

---
