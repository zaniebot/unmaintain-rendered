```yaml
number: 2696
title: "Use `Resolver` in `pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/finder
created_at: 2024-03-27T17:22:56Z
updated_at: 2024-03-27T21:36:39Z
url: https://github.com/astral-sh/uv/pull/2696
synced_at: 2026-01-10T14:49:08Z
```

# Use `Resolver` in `pip sync`

---

_Pull request opened by @charliermarsh on 2024-03-27 17:22_

## Summary

This PR removes the custom `DistFinder` that we use in `pip sync`. This originally existed because `VersionMap` wasn't lazy, and so we saved a lot of time in `DistFinder` by reading distribution data lazily. But now, AFAICT, there's really no benefit. Maintaining `DistFinder` means we effectively have to maintain two resolvers, and end up fixing bugs in `DistFinder` that don't exist in the `Resolver` (like #2688.

Closes #2694.

Closes #2443.

## Test Plan

I ran this benchmark a bunch. It's basically a wash. Sometimes one is faster than the other.

```
❯ python -m scripts.bench \
        --uv-path ./target/release/main \
        --uv-path ./target/release/uv \
        scripts/requirements/compiled/trio.txt --min-runs 50 --benchmark install-warm --warmup 25
Benchmark 1: ./target/release/main (install-warm)
  Time (mean ± σ):      54.0 ms ±  10.6 ms    [User: 8.7 ms, System: 98.1 ms]
  Range (min … max):    45.5 ms …  94.3 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/uv (install-warm)
  Time (mean ± σ):      50.7 ms ±   9.2 ms    [User: 8.7 ms, System: 98.6 ms]
  Range (min … max):    44.0 ms …  98.6 ms    50 runs

  Warning: The first benchmarking run for this command was significantly slower than the rest (77.6 ms). This could be caused by (filesystem) caches that were not filled until after the first run. You should consider using the '--warmup' option to fill those caches before the actual benchmark. Alternatively, use the '--prepare' option to clear the caches before each timing run.

Summary
  './target/release/uv (install-warm)' ran
    1.06 ± 0.29 times faster than './target/release/main (install-warm)'
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-27 17:23_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-27 17:23_

---

_@charliermarsh reviewed on 2024-03-27 17:23_

---

_Review comment by @charliermarsh on `crates/uv-dev/src/main.rs`:69 on 2024-03-27 17:23_

Does anyone care about preserving this? I can migrate it but I wasn't sure if it was worth it. \cc @konstin 

---

_Marked ready for review by @charliermarsh on 2024-03-27 17:24_

---

_@charliermarsh reviewed on 2024-03-27 17:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2593 on 2024-03-27 17:25_

This is an improvement, I think.

---

_@charliermarsh reviewed on 2024-03-27 17:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:901 on 2024-03-27 17:25_

This is an improvement, I think.

---

_@charliermarsh reviewed on 2024-03-27 17:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:587 on 2024-03-27 17:25_

This is an improvement, I think.

---

_@charliermarsh reviewed on 2024-03-27 17:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1806 on 2024-03-27 17:27_

I don't know why this changed. Will look into it.

---

_@charliermarsh reviewed on 2024-03-27 17:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2988 on 2024-03-27 17:27_

This was https://github.com/astral-sh/uv/issues/2443.

---

_@zanieb reviewed on 2024-03-27 17:32_

---

_Review comment by @zanieb on `crates/uv-dev/src/main.rs`:69 on 2024-03-27 17:32_

I've been annoyed maintaining it since I don't use it.

---

_@zanieb approved on 2024-03-27 17:33_

This makes sense to me. Will have conflicts with #2596, we'll need to add support for local site packages to `pip sync`.

---

_@konstin reviewed on 2024-03-27 17:50_

---

_Review comment by @konstin on `crates/uv-dev/src/main.rs`:69 on 2024-03-27 17:50_

It was nice to have but honestly not if its effort to maintain, i don't run it anymore

---

_@BurntSushi approved on 2024-03-27 18:15_

Wow.

---

_@charliermarsh reviewed on 2024-03-27 20:41_

---

_Review comment by @charliermarsh on `crates/uv-dev/src/main.rs`:69 on 2024-03-27 20:41_

Haha. If I had my way I would remove them all.

---

_@charliermarsh reviewed on 2024-03-27 20:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1806 on 2024-03-27 20:53_

We have an additional `.msgpack` file from the cache, because we fetch the `Requires-Python` for the URL.

---

_Merged by @charliermarsh on 2024-03-27 21:36_

---

_Closed by @charliermarsh on 2024-03-27 21:36_

---

_Branch deleted on 2024-03-27 21:36_

---
