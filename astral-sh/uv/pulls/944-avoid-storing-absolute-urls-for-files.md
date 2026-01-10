```yaml
number: 944
title: Avoid storing absolute URLs for files
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/url
created_at: 2024-01-16T22:32:09Z
updated_at: 2024-01-17T14:15:22Z
url: https://github.com/astral-sh/uv/pull/944
synced_at: 2026-01-10T15:39:02Z
```

# Avoid storing absolute URLs for files

---

_Pull request opened by @charliermarsh on 2024-01-16 22:32_

## Summary

It turns out that storing an absolute URL for every file caused a significant performance regression. This PR attempts to address the regression with two changes.

The first is that we now store the raw string if the URL is an absolute URL. If the URL is relative, we store the base URL alongside the raw relative string. As such, we avoid serializing and deserializing URLs until we need them (later on), except for the base URL.

The second is that we now use the internal `Url` crate methods for serializing and deserializing. If you look inside `Url`, its standard serializer and deserialization actually convert it to a string, then parse the string. But the crate exposes some other methods for faster serialization and deserialization (with fewer guarantees). I think this is totally fine since the cache is entirely internal.

If we _just_ change the `Url` serialization (and no other code -- so continue to store URLs for every file), then the regression goes down to about 5%:

```shell
❯ python -m scripts.bench \
        --puffin-path ./target/release/main \
        --puffin-path ./target/release/relative --puffin-path ./target/release/puffin \
        scripts/requirements/home-assistant.in --benchmark resolve-warm
Benchmark 1: ./target/release/main (resolve-warm)
  Time (mean ± σ):     496.3 ms ±   4.3 ms    [User: 452.4 ms, System: 175.5 ms]
  Range (min … max):   487.3 ms … 502.4 ms    10 runs

Benchmark 2: ./target/release/relative (resolve-warm)
  Time (mean ± σ):     284.8 ms ±   2.1 ms    [User: 245.8 ms, System: 165.6 ms]
  Range (min … max):   280.3 ms … 288.0 ms    10 runs

Benchmark 3: ./target/release/puffin (resolve-warm)
  Time (mean ± σ):     300.4 ms ±   3.2 ms    [User: 255.5 ms, System: 178.1 ms]
  Range (min … max):   295.4 ms … 305.1 ms    10 runs

Summary
  './target/release/relative (resolve-warm)' ran
    1.05 ± 0.01 times faster than './target/release/puffin (resolve-warm)'
    1.74 ± 0.02 times faster than './target/release/main (resolve-warm)'
```

So I considered _just_ making that change. But 5% is kind of borderline...

With both of these changes, the regression is down to 1-2%:

```
Benchmark 1: ./target/release/relative (resolve-warm)
  Time (mean ± σ):     282.6 ms ±   7.4 ms    [User: 244.6 ms, System: 181.3 ms]
  Range (min … max):   275.1 ms … 318.5 ms    30 runs

Benchmark 2: ./target/release/puffin (resolve-warm)
  Time (mean ± σ):     286.8 ms ±   2.2 ms    [User: 247.0 ms, System: 169.1 ms]
  Range (min … max):   282.3 ms … 290.7 ms    30 runs

Summary
  './target/release/relative (resolve-warm)' ran
    1.01 ± 0.03 times faster than './target/release/puffin (resolve-warm)'
```

It's consistently ~2%-ish, but at this point it's unclear if that's due to the URL change or something other change between now and then.

Closes #943.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-16 22:32_

---

_Review requested from @konstin by @charliermarsh on 2024-01-16 22:32_

---

_Label `performance` added by @charliermarsh on 2024-01-16 22:32_

---

_@charliermarsh reviewed on 2024-01-16 22:33_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:731 on 2024-01-16 22:33_

It's perhaps worth noting that these are called very infrequently... You have to have distributions without any hashes.

---

_@konstin approved on 2024-01-17 08:51_

---

_@BurntSushi approved on 2024-01-17 13:15_

I think this LGTM.

---

_Merged by @charliermarsh on 2024-01-17 14:15_

---

_Closed by @charliermarsh on 2024-01-17 14:15_

---

_Branch deleted on 2024-01-17 14:15_

---
