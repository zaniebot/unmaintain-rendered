```yaml
number: 2628
title: Add fast build profile without debuginfo
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fast-build
created_at: 2024-03-22T23:12:56Z
updated_at: 2024-03-26T11:15:45Z
url: https://github.com/astral-sh/uv/pull/2628
synced_at: 2026-01-10T14:49:08Z
```

# Add fast build profile without debuginfo

---

_Pull request opened by @konstin on 2024-03-22 23:12_

Add a compile option `-p fast-build` for a 16% incremental compile speedup (linux) at the cost of having no debuginfo.

After trying various rust compiler speedup suggestions, setting mold as my default linker and removing debuginfo are the only ones showing a speedup.

```
hyperfine --warmup 1 --runs 3 --prepare "touch crates/uv-resolver/src/resolver/mod.rs" \
    "cargo +nightly build --bin uv" \
    "cargo +nightly build --bin uv --profile fast-build"
Benchmark 1: cargo +nightly build --bin uv
  Time (mean ± σ):      1.569 s ±  0.008 s    [User: 1.179 s, System: 0.369 s]
  Range (min … max):    1.560 s …  1.576 s    3 runs

Benchmark 2: cargo +nightly build --bin uv --profile fast-build
  Time (mean ± σ):      1.353 s ±  0.020 s    [User: 1.109 s, System: 0.301 s]
  Range (min … max):    1.338 s …  1.375 s    3 runs

Summary
  cargo +nightly build --bin uv --profile fast-build ran
    1.16 ± 0.02 times faster than cargo +nightly build --bin uv
```

---

_Label `internal` added by @konstin on 2024-03-22 23:12_

---

_Comment by @charliermarsh on 2024-03-23 00:02_

What's the use-case for this?

---

_Comment by @zanieb on 2024-03-23 01:20_

Should we use this in CI?

---

_Comment by @konstin on 2024-03-23 07:49_

> What's the use-case for this?

A faster edit-run loop, ideally

---

_Comment by @charliermarsh on 2024-03-23 13:35_

What is the practical cost of having no debug info? (My main pushback here is: who's going to remember to set this? When should they? Should this be the default? Etc.)

---

_Comment by @konstin on 2024-03-25 10:37_

There were several reports recently about massive speedups for debug builds using advanced or experimental cargo options: Using the parallel frontend, the cranelift backend, static musl, etc. I found that none of them worked for us except using mold (which i have in my global configuration) and removing debuginfo.

This PR adds `-p fast-build` to cargo commands for a speedup when changing a file i commonly work on. No debuginfo means that tracebacks will be empty and you can't use an debugger on the artifact. Since i need either rarely i find this a free 16% speedup a good tradeoff. I made this a separate profile to avoid confusing people who expect the default debug build to have panic traceback and work with their debuggers.

> Should we use this in CI?

We have to try! I haven't checked how much it matters for that case

---

_@charliermarsh approved on 2024-03-25 14:23_

---

_Merged by @charliermarsh on 2024-03-25 20:57_

---

_Closed by @charliermarsh on 2024-03-25 20:57_

---

_Branch deleted on 2024-03-25 20:57_

---

_Comment by @charliermarsh on 2024-03-25 20:57_

Lets give it a try.

---

_Comment by @charliermarsh on 2024-03-25 20:57_

Can I make this my default somehow?

---

_Comment by @konstin on 2024-03-26 11:15_

> Can I make this my default somehow?

Except for aliases, i don't think so unfortunately

---
