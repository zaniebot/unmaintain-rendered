```yaml
number: 1157
title: Stream unpacking of source distribution downloads
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/async-zip
created_at: 2024-01-29T00:39:08Z
updated_at: 2024-01-29T12:33:47Z
url: https://github.com/astral-sh/uv/pull/1157
synced_at: 2026-01-12T16:04:28Z
```

# Stream unpacking of source distribution downloads

---

_@charliermarsh_

This PR migrates our source distribution downloads to unzip as we stream, similar to our approach for wheels.

In my testing, this showed a consistent speedup (e.g., 6% here for a few representative source distributions):

```text
❯ python -m scripts.bench --puffin-path ./target/release/main --puffin-path ./target/release/puffin --benchmark install-cold requirements.in
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):      1.503 s ±  0.039 s    [User: 1.479 s, System: 0.537 s]
  Range (min … max):    1.466 s …  1.605 s    10 runs

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):      1.421 s ±  0.024 s    [User: 1.505 s, System: 0.593 s]
  Range (min … max):    1.381 s …  1.454 s    10 runs

Summary
  './target/release/puffin (install-cold)' ran
    1.06 ± 0.03 times faster than './target/release/main (install-cold)'
```

---

_Label `performance` added by @charliermarsh on 2024-01-29 00:39_

---

_Merged by @charliermarsh on 2024-01-29 01:09_

---

_Closed by @charliermarsh on 2024-01-29 01:09_

---

_Branch deleted on 2024-01-29 01:09_

---

_@konstin reviewed on 2024-01-29 07:49_

---

_Review comment by @konstin on `Cargo.toml`:23 on 2024-01-29 07:49_

I'm a bit worried about the async-std dep, it's abandoned and could clash with tokio(?)

---

_@charliermarsh reviewed on 2024-01-29 12:03_

---

_Review comment by @charliermarsh on `Cargo.toml`:23 on 2024-01-29 12:03_

What is your source for `async-std` being abandoned?

---

_@charliermarsh reviewed on 2024-01-29 12:05_

---

_Review comment by @charliermarsh on `Cargo.toml`:23 on 2024-01-29 12:05_

I can migrate to https://crates.io/crates/tokio-tar which is a fork for Tokio. Or I can look into forking `async-tar` to land this PR: https://github.com/dignifiedquire/async-tar/pull/41.

---

_@konstin reviewed on 2024-01-29 12:33_

---

_Review comment by @konstin on `Cargo.toml`:23 on 2024-01-29 12:33_

There is no official message or anything, there's just barely any activity on the repo anymore

![image](https://github.com/astral-sh/puffin/assets/6826232/1ab6aec7-7754-49d6-8eb5-fea0e9fe91d0)

![image](https://github.com/astral-sh/puffin/assets/6826232/10a68946-7caa-40b4-9eae-7d1789264825)

As long as it's stable it's fine, i'm more worried we'll get some clash with tokio or something because they are different executors with different runtimes and types.

---
