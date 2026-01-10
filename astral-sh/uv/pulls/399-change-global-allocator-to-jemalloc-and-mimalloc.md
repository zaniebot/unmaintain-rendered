```yaml
number: 399
title: change global allocator to jemalloc (and mimalloc on Windows)
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/change-global-allocator
created_at: 2023-11-10T19:34:36Z
updated_at: 2024-06-29T16:31:44Z
url: https://github.com/astral-sh/uv/pull/399
synced_at: 2026-01-10T13:48:28Z
```

# change global allocator to jemalloc (and mimalloc on Windows)

---

_Pull request opened by @BurntSushi on 2023-11-10 19:34_

This copies the allocator configuration used in the Ruff project. In
particular, this gives us an instant 10% win when resolving the top 1K
PyPI packages:

    $ hyperfine \
        "./target/profiling/puffin-dev-main resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null" \
        "./target/profiling/puffin-dev resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null"
    Benchmark 1: ./target/profiling/puffin-dev-main resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null
      Time (mean ± σ):     974.2 ms ±  26.4 ms    [User: 17503.3 ms, System: 2205.3 ms]
      Range (min … max):   943.5 ms … 1015.9 ms    10 runs

    Benchmark 2: ./target/profiling/puffin-dev resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null
      Time (mean ± σ):     883.1 ms ±  23.3 ms    [User: 14626.1 ms, System: 2542.2 ms]
      Range (min … max):   849.5 ms … 916.9 ms    10 runs

    Summary
      './target/profiling/puffin-dev resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null' ran
        1.10 ± 0.04 times faster than './target/profiling/puffin-dev-main resolve-many --cache-dir cache-docker-no-build --no-build pypi_top_8k_flat.txt --limit 1000 2> /dev/null'

I was moved to do this because I noticed `malloc`/`free` taking up a
fairly sizeable percentage of time during light profiling.

As is becoming a pattern, it will be easier to review this commit-by-commit.

Ref #396 (wouldn't call this issue fixed)

-----

I did also try adding a `smallvec` optimization to the `Version::release` field, but it didn't bare any fruit. I still think there is more to explore since the results I observed don't quite line up with what I expect. (So probably either my mental model is off or my measurement process is flawed.) You can see that attempt with a little more explanation here: https://github.com/astral-sh/puffin/commit/f9528b4ecd1b0c260df7e8ad57b9ddc4da09d273

In the course of adding the `smallvec` optimization, I also shrunk the `Version` fields from a `usize` to a `u32`. They should at least be a fixed size integer since version numbers aren't used to index memory, and I shrunk it to `u32` since it seems reasonable to assume that all version numbers will be smaller than `2^32`.

---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-10 19:34_

---

_Review requested from @konstin by @BurntSushi on 2023-11-10 19:34_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:1229 on 2023-11-10 19:36_

Dumb question, what is the \x20 here?

---

_@charliermarsh reviewed on 2023-11-10 19:36_

---

_@charliermarsh approved on 2023-11-10 19:36_

Sweet.

---

_Comment by @BurntSushi on 2023-11-10 19:36_

Note that this PR also seems to result in version parsing taking up a larger percentage of time. So the potential win from doing #373 has gone up (relatively speaking).

---

_@charliermarsh reviewed on 2023-11-10 19:37_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:216 on 2023-11-10 19:37_

Perhaps worth a footnote in the PR summary that we _did_ keep this change (since the summary only mentions `smallvec` right now).

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/lib.rs`:1229 on 2023-11-10 19:37_

I mentioned that in the commit message. It's an ASCII space character. My editor removes all trailing whitespace from every line.

---

_@BurntSushi reviewed on 2023-11-10 19:37_

---

_@BurntSushi reviewed on 2023-11-10 19:38_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/lib.rs`:1229 on 2023-11-10 19:38_

Using an explicit escape means that my editor won't remove it.

Not a great situation I admit, but removing trailing whitespace automatically is really really useful. Hopefully these are rare enough to not be too much of an issue.

---

_@BurntSushi reviewed on 2023-11-10 19:39_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:216 on 2023-11-10 19:39_

Yeah I mentioned that in the commit messages too, but I suppose those will get dropped upon squashing... I'll update the PR summary.

---

_Merged by @BurntSushi on 2023-11-10 19:49_

---

_Closed by @BurntSushi on 2023-11-10 19:49_

---

_Branch deleted on 2023-11-10 19:49_

---

_@konstin reviewed on 2023-11-10 19:55_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:1229 on 2023-11-10 19:55_

I think the best is to remove the trailing space from the input here

---

_Comment by @konstin on 2023-11-10 19:58_

For smallvec, does `SmallVec<[u32; 3]>` give better results? Most versions are exactly three long and the resulting datatype should fit in 16 byte

---

_@charliermarsh reviewed on 2023-11-10 19:59_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:216 on 2023-11-10 19:59_

I swear I did read the commit message :)

---

_Comment by @BurntSushi on 2023-11-10 19:59_

> For smallvec, does `SmallVec<[u32; 3]>` give better results? Most versions are exactly three long and the resulting datatype should fit in 16 byte

I tried that and `SmallVec<[u32; 4]>`. Couldn't observe a difference. I ended up at `8` as a sanity check to ensure heap allocation really wasn't happening.

---

_@BurntSushi reviewed on 2023-11-10 20:00_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/lib.rs`:1229 on 2023-11-10 20:00_

Wow. Derp. I did not see that.

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:216 on 2023-11-10 20:01_

:P

---

_@BurntSushi reviewed on 2023-11-10 20:01_

---

_Comment by @adminy on 2024-06-29 16:24_

```
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 5 bytes failed
[1]    21521 abort (core dumped)  uv venv
```

---

_Comment by @zanieb on 2024-06-29 16:31_

@adminy please open a new issue with reproduction details if you want us to investigate.

---
