```yaml
number: 3688
title: "Arc-wrap `PubGrubPackage` for cheap cloning in pubgrub"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/arc-package
created_at: 2024-05-21T10:11:26Z
updated_at: 2024-05-21T11:51:26Z
url: https://github.com/astral-sh/uv/pull/3688
synced_at: 2026-01-12T16:05:48Z
```

# Arc-wrap `PubGrubPackage` for cheap cloning in pubgrub

---

_@konstin_

Pubgrub stores incompatibilities as (package name, version range) tuples, meaning it needs to clone the package name for each incompatibility, and each non-borrowed operation on incompatibilities. https://github.com/astral-sh/uv/pull/3673 made me realize that `PubGrubPackage` has gotten large (expensive to copy), so like `Version` and other structs, i've added an `Arc` wrapper around it.

It's a pity clippy forbids `.deref()`, it's less opaque than `&**` and has IDE support (clicking on `.deref()` jumps to the right impl).

## Benchmarks

It looks like this matters most for complex resolutions which, i assume because they carry larger `PubGrubPackageInner::Package` and `PubGrubPackageInner::Extra` types.

```bash
hyperfine --warmup 5 "./uv-main pip compile -q ./scripts/requirements/jupyter.in" "./uv-branch pip compile -q ./scripts/requirements/jupyter.in"
hyperfine --warmup 5 "./uv-main pip compile -q ./scripts/requirements/airflow.in" "./uv-branch pip compile -q ./scripts/requirements/airflow.in"
hyperfine --warmup 5 "./uv-main pip compile -q ./scripts/requirements/boto3.in" "./uv-branch pip compile -q ./scripts/requirements/boto3.in"
```

```
Benchmark 1: ./uv-main pip compile -q ./scripts/requirements/jupyter.in
  Time (mean ± σ):      18.2 ms ±   1.6 ms    [User: 14.4 ms, System: 26.0 ms]
  Range (min … max):    15.8 ms …  22.5 ms    181 runs

Benchmark 2: ./uv-branch pip compile -q ./scripts/requirements/jupyter.in
  Time (mean ± σ):      17.8 ms ±   1.4 ms    [User: 14.4 ms, System: 25.3 ms]
  Range (min … max):    15.4 ms …  23.1 ms    159 runs

Summary
  ./uv-branch pip compile -q ./scripts/requirements/jupyter.in ran
    1.02 ± 0.12 times faster than ./uv-main pip compile -q ./scripts/requirements/jupyter.in
```

```
Benchmark 1: ./uv-main pip compile -q ./scripts/requirements/airflow.in
  Time (mean ± σ):     153.7 ms ±   3.5 ms    [User: 165.2 ms, System: 157.6 ms]
  Range (min … max):   150.4 ms … 163.0 ms    19 runs

Benchmark 2: ./uv-branch pip compile -q ./scripts/requirements/airflow.in
  Time (mean ± σ):     123.9 ms ±   4.6 ms    [User: 152.4 ms, System: 133.8 ms]
  Range (min … max):   118.4 ms … 138.1 ms    24 runs

Summary
  ./uv-branch pip compile -q ./scripts/requirements/airflow.in ran
    1.24 ± 0.05 times faster than ./uv-main pip compile -q ./scripts/requirements/airflow.in
```

```
Benchmark 1: ./uv-main pip compile -q ./scripts/requirements/boto3.in
  Time (mean ± σ):     327.0 ms ±   3.8 ms    [User: 344.5 ms, System: 71.6 ms]
  Range (min … max):   322.7 ms … 334.6 ms    10 runs

Benchmark 2: ./uv-branch pip compile -q ./scripts/requirements/boto3.in
  Time (mean ± σ):     311.2 ms ±   3.1 ms    [User: 339.3 ms, System: 63.1 ms]
  Range (min … max):   307.8 ms … 317.0 ms    10 runs

Summary
  ./uv-branch pip compile -q ./scripts/requirements/boto3.in ran
    1.05 ± 0.02 times faster than ./uv-main pip compile -q ./scripts/requirements/boto3.in
```

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


---

_Label `performance` added by @konstin on 2024-05-21 10:11_

---

_Review requested from @BurntSushi by @konstin on 2024-05-21 10:11_

---

_@BurntSushi approved on 2024-05-21 11:40_

I think you read my mind. I was thinking about doing exactly this too. I don't think I realized how much we were cloning `PubGrubPackage` though. Nice find. This will probably help even more once `marker` is a non-`None` value. (Right now, it's always `None`, so cloning will still be cheap.)

While I don't mind the `&*foo` and `&**foo` myself, if you wanted to go the `foo.deref()` route, I'd recommend just defining an additional concrete method, e.g., `foo.as_inner()` or something.

---

_Merged by @konstin on 2024-05-21 11:49_

---

_Closed by @konstin on 2024-05-21 11:49_

---

_Branch deleted on 2024-05-21 11:49_

---

_Comment by @charliermarsh on 2024-05-21 11:51_

Thanks!

---
