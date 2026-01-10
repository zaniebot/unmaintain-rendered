```yaml
number: 11894
title: "Migrate to `zlib-rs` (again)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - no-build
assignees: []
merged: true
base: main
head: charlie/bench-zlib
created_at: 2025-03-02T17:01:08Z
updated_at: 2025-03-03T17:29:32Z
url: https://github.com/astral-sh/uv/pull/11894
synced_at: 2026-01-10T11:10:39Z
```

# Migrate to `zlib-rs` (again)

---

_Pull request opened by @charliermarsh on 2025-03-02 17:01_

## Summary

I believe `zlib-rs` is now a better choice on ARM and x86, so I'm just going to assume it's a better choice everywhere. It's much easier to build (removes our CMake dependency), and in my benchmarking, it's substantially faster on ARM and faster or ~exactly even on my x86 Windows machine.

We migrated to `zlib-rs` once before (#9184); however, I later reverted it as I learned that they were only doing compile-time feature detection, and so `zlib-rs` was meaningfully slower on x86. They now perform runtime feature detection: https://trifectatech.org/blog/zlib-rs-is-faster-than-c/.

To benchmark, I wrote a script to create a local Simple API-compliant registry (see the commit history) for a single package. Then I ran the `install-cold` benchmark against that registry to install NumPy.

On ARM:

```
❯ uv run resolver --uv-pip-path ../../zlib-ng --uv-pip-path ../../zlib-rs \
        --benchmark install-cold \
        req.txt --warmup 10 --min-runs 30
Benchmark 1: ../../zlib-ng (install-cold)
  Time (mean ± σ):     165.7 ms ±  34.7 ms    [User: 64.4 ms, System: 93.2 ms]
  Range (min … max):   141.8 ms … 293.2 ms    30 runs

Benchmark 2: ../../zlib-rs (install-cold)
  Time (mean ± σ):     150.9 ms ±  16.2 ms    [User: 57.4 ms, System: 86.4 ms]
  Range (min … max):   135.3 ms … 202.4 ms    30 runs

Summary
  ../../zlib-rs (install-cold) ran
    1.10 ± 0.26 times faster than ../../zlib-ng (install-cold)
```

I benchmarked this about 100 times on my Windows machine and found it difficult to conclude anything beyond "They're nearly the same". Here's an example:

```
PS C:\Users\crmar\workspace\puffin> hyperfine --prepare "uv venv" "zlib-rs.exe pip sync ./scripts/benchmark/req.txt" "zlib-ng.exe pip sync ./scripts/benchmark/req.txt" "zlib-rs.exe pip sync ./scripts/benchmark/req.txt" "zlib-ng.exe pip sync ./scripts/benchmark/req.txt" --runs 10 --warmup 5
Benchmark 1: zlib-rs.exe pip sync ./scripts/benchmark/req.txt
  Time (mean ± σ):     240.6 ms ±  10.8 ms    [User: 6.1 ms, System: 92.2 ms]
  Range (min … max):   229.4 ms … 267.9 ms    10 runs

Benchmark 2: zlib-ng.exe pip sync ./scripts/benchmark/req.txt
  Time (mean ± σ):     241.3 ms ±   6.2 ms    [User: 7.7 ms, System: 90.6 ms]
  Range (min … max):   233.9 ms … 252.1 ms    10 runs

Benchmark 3: zlib-rs.exe pip sync ./scripts/benchmark/req.txt
  Time (mean ± σ):     242.8 ms ±   7.7 ms    [User: 6.2 ms, System: 23.4 ms]
  Range (min … max):   236.1 ms … 262.8 ms    10 runs

Benchmark 4: zlib-ng.exe pip sync ./scripts/benchmark/req.txt
  Time (mean ± σ):     245.9 ms ±   5.7 ms    [User: 1.5 ms, System: 59.4 ms]
  Range (min … max):   240.9 ms … 257.3 ms    10 runs

Summary
  zlib-rs.exe pip sync ./scripts/benchmark/req.txt ran
    1.00 ± 0.05 times faster than zlib-ng.exe pip sync ./scripts/benchmark/req.txt
    1.01 ± 0.06 times faster than zlib-rs.exe pip sync ./scripts/benchmark/req.txt
    1.02 ± 0.05 times faster than zlib-ng.exe pip sync ./scripts/benchmark/req.txt
```

Closes #11885.


---

_Label `performance` added by @charliermarsh on 2025-03-02 17:04_

---

_Marked ready for review by @charliermarsh on 2025-03-02 17:04_

---

_Comment by @zanieb on 2025-03-02 19:14_

Can we drop cmake from the contributing guide then?

---

_Comment by @charliermarsh on 2025-03-02 19:20_

Sure... Do you know if a C compiler is still "required"? The instructions seem sort of ancient.

---

_Review requested from @BurntSushi by @charliermarsh on 2025-03-03 02:19_

---

_Review requested from @konstin by @charliermarsh on 2025-03-03 02:19_

---

_Label `no-build` added by @charliermarsh on 2025-03-03 02:19_

---

_Comment by @konstin on 2025-03-03 10:30_

On a `ubuntu` container, with this PR, you need only `gcc` and `make` to build uv. It successfully removes the cmake dependency, so you only need the `sudo apt install build-essential` you need for building ~everything.

---

_@konstin approved on 2025-03-03 10:30_

---

_@BurntSushi approved on 2025-03-03 12:43_

Love it!

---

_Comment by @charliermarsh on 2025-03-03 15:53_

Thanks @konstin. I'll restore the instructions around `build-essential`.

---

_Comment by @konstin on 2025-03-03 17:11_

For me it's a question of who we're targeting: Anyone with dev experience will have `build-essential` installed as you hit when compiling almost anything, if we're targeting beginners (i think we have some contributing to uv), it helps being explicit on "`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh && sudo apt install build-essential` and you're ready to develop".

---

_Comment by @charliermarsh on 2025-03-03 17:22_

I left it in for now, just seems unrelated to this change to remove it.

---

_Merged by @charliermarsh on 2025-03-03 17:29_

---

_Closed by @charliermarsh on 2025-03-03 17:29_

---

_Branch deleted on 2025-03-03 17:29_

---
