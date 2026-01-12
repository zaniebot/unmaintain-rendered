```yaml
number: 5904
title: Enable LTO optimizations in release builds to reduce binary size
type: pull_request
state: merged
author: davfsa
labels:
  - performance
assignees: []
merged: true
base: main
head: task/enable-lto
created_at: 2024-08-08T11:27:08Z
updated_at: 2025-04-30T12:55:25Z
url: https://github.com/astral-sh/uv/pull/5904
synced_at: 2026-01-12T16:07:05Z
```

# Enable LTO optimizations in release builds to reduce binary size

---

_@davfsa_

## Summary

In the same spirit as #5745, release builds could be a bit slightly more size efficient by enabling LTO, which removes dead code (either in uv through fully inlined functions or the libraries it depends on). Also has the side-effect (more what LTO was created for) of slighly speeding up uv.

In this case, I have measured a 5MB size decrease!.

Unfortunately, this change also comes with the disadvantage of more than doubling the build time of a clean build on my machine (see "Test Plan"). I have opened this pull request to show my findings and suggest this as an option. 

*I have also started looking into what effects optimizing for size rather than speed could have, but that calls for another pr*

## Test Plan

Comparing the binary size before and after (starting off in just a simple clone of the repository)

System info:
```
CPU: AMD Ryzen 7 3700X (16) @ 3.600GHz
Memory: 32GB @ 3200 MT/s
Uname: Linux galaxy 6.6.44-1-MANJARO #1 SMP PREEMPT_DYNAMIC Sat Aug  3 10:09:33 UTC 2024 x86_64 GNU/Linux
```

Before:
```
$ cargo build --release
<snip>
Finished `release` profile [optimized] target(s) in 1m 29s

$ du target/release/uv -h
30M	target/release/uv
```

After:
```
$ cargo build --release
<snip>
Finished `release` profile [optimized] target(s) in 3m 43s

$ du target/release/uv -h
25M	target/release/uv
```


---

_Renamed from "Enable LTO optimizations to decrease binary size" to "Enable LTO optimizations in release builds to reduce binary size" by @davfsa on 2024-08-08 11:27_

---

_Comment by @codspeed-hq[bot] on 2024-08-08 11:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/davfsa:task/enable-lto)

### Merging #5904 will **improve performances by 14.9%**

<sub>Comparing <code>davfsa:task/enable-lto</code> (537ecf6) with <code>main</code> (acbd367)</sub>



### Summary

`⚡ 6` improvements
`✅ 7` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `davfsa:task/enable-lto` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `build_platform_tags[burntsushi-archlinux]` | 1.3 ms | 1.1 ms | +13.69% |
| ⚡ | `wheelname_parsing[flyte-long-compatible]` | 10 µs | 8.8 µs | +12.91% |
| ⚡ | `wheelname_parsing[flyte-short-compatible]` | 6.3 µs | 5.5 µs | +14.54% |
| ⚡ | `wheelname_parsing[flyte-short-incompatible]` | 6.3 µs | 5.5 µs | +14.28% |
| ⚡ | `wheelname_parsing_failure[flyte-long-extension]` | 1.9 µs | 1.6 µs | +14.9% |
| ⚡ | `wheelname_parsing_failure[flyte-short-extension]` | 1.9 µs | 1.7 µs | +14.64% |


---

_Comment by @konstin on 2024-08-08 11:41_

I'm seeing a small speedup, too:

```
hyperfine --prepare "rm -f uv.lock" --warmup 3 --runs 30 "~/projects/uv-main/uv-main lock" "~/projects/uv-main/uv-lto lock"
Benchmark 1: ~/projects/uv-main/uv-main lock
  Time (mean ± σ):     218.1 ms ±   3.6 ms    [User: 217.5 ms, System: 129.5 ms]
  Range (min … max):   212.5 ms … 227.1 ms    30 runs
 
Benchmark 2: ~/projects/uv-main/uv-lto lock
  Time (mean ± σ):     208.5 ms ±   4.3 ms    [User: 204.8 ms, System: 128.0 ms]
  Range (min … max):   201.7 ms … 222.1 ms    30 runs
```

---

_@konstin approved on 2024-08-08 11:41_

---

_Label `performance` added by @konstin on 2024-08-08 11:41_

---

_Comment by @T-256 on 2024-08-08 11:47_

- related discussion: https://github.com/astral-sh/ruff/issues/9224

> Comparing the binary size before and after (starting off in just a simple clone of the repository)

Can you also share final released wheels size (compressed)?

---

_Comment by @davfsa on 2024-08-08 11:59_

> Can you also share final released wheels size (compressed)?

Absolutely! Currently struggling to find how to build those (can't find any job where that is done and the [release-pypi ci](https://github.com/astral-sh/uv/blob/main/.github/workflows/publish-pypi.yml) eludes to `cargo-dist` being what I need, but I have found no documentation on how to generate only the wheels

---

_Comment by @konstin on 2024-08-08 11:59_

You can use `maturin build --release` (or even `uvx maturin build --release`)

---

_Comment by @davfsa on 2024-08-08 12:09_

Thanks @konstin!

@T-256 Here are your results (a bit underwhelming actually):

Before:
```
$ du targets/wheels/uv-0.2.34-py3-none-linux_x86_64.whl -h
12M	uv-0.2.34-py3-none-linux_x86_64.whl
```

After:
```
$ du target/wheels/uv-0.2.34-py3-none-linux_x86_64.whl -h
11M	target/wheels/uv-0.2.34-py3-none-linux_x86_64.whl
```

Btw, thanks for sharing the discussion about this in the ruff repository! If necessary, I can spend some time playing around a bit with build optimizations to make it take less time to compile release builds. Build times did take a massive hit (at least on my machine, from ~1m to ~3m on a fresh build).

Just through a quick read of discussion you linked and the other ones linked by other people, it seems like there is a lot more to consider here than I initially thought.

---

_Merged by @charliermarsh on 2024-08-08 12:23_

---

_Closed by @charliermarsh on 2024-08-08 12:23_

---

_Comment by @charliermarsh on 2024-08-08 12:24_

I'm supportive of this. Saving on binary size is a win for the registry, CDN, and users. We can always revisit in the future.

---

_Comment by @BurntSushi on 2024-08-08 13:25_

I would say I'm somewhat mildly negative on this change, largely for the same reasons I discussed here for `ruff`: https://github.com/astral-sh/ruff/pull/9031

Building `uv` already takes a very long time, and doubling its build time for small improvements to binary size and runtime speed doesn't seem like the best trade off to me. Although the improvement to binary size does look pretty good here. Compounded over lots of downloads, that could really add up.

With that said, I'm already using `--profile profiling` locally when building `uv` for benchmarking, which is different from the `release` configuration (but only insomuch as debug symbols aren't stripped). With this change, the configurations differ even more now, with optimizations potentially being different between the builds. This means that when we're doing benchmarking locally, we'll be measuring the thing we aren't actually shipping. It is very difficult for me to estimate just how big of a deal that is. It's probably _not_ a huge deal in the vast majority of circumstances. But it's a concern I have.

I'm not _strongly_ opposed to this change though. So I fine with keeping it for now and evaluating just how much the longer build times annoy us.

---

_Comment by @davfsa on 2024-08-08 14:14_

First of all, thanks for joining into the conversation @BurntSushi. Your comments on https://github.com/astral-sh/ruff/issues/9224 and similar were insanely insightful!

> With that said, I'm already using --profile profiling locally when building uv for benchmarking, which is different from the release configuration (but only insomuch as debug symbols aren't stripped). With this change, the configurations differ even more now, with optimizations potentially being different between the builds. This means that when we're doing benchmarking locally, we'll be measuring the thing we aren't actually shipping.

The `profiling` profile inherits from the `release` one, so I believe it should have `lto` enabled too (not a cargo expert). Then the only differences would be those that are specifically marked (being not stripping the binary and enabling debug at the time of writing)

I have also opened #5909 to try to further optimize binary size without giving up too much performance nor build times :)

---

_Comment by @BurntSushi on 2024-08-08 14:28_

> The profiling profile inherits from the release one, so I believe it should have lto enabled too (not a cargo expert).

Oof. Right. That needs to be undone. It is not tenable to wait minutes between benchmarking runs IMO. (The compile times are already too long.)

---

_Comment by @davfsa on 2024-08-08 14:33_

> Oof. Right. That needs to be undone. It is not tenable to wait minutes between benchmarking runs IMO. (The compile times are already too long.)

I'm sorry for the build slowdowns! Didn't initially think they were done for anything else than a release, where I thought it would be fine to give up some build time to reduce the final binary.

If #5909 is not good enough (from a clean build, it now only takes about 30s more than before and 1m 45s less than with LTO only, at a slight performance cost that is), I would understand this PR being reverted

---

_Comment by @BurntSushi on 2024-08-08 14:42_

I don't think we need to revert the PR. I think we just need to make the `profiling` profile not inherit from `release`.

---

_Comment by @zanieb on 2024-08-09 23:12_

This change was not tested for all of our release builds unfortunately, it's failing in #5984 — considering reverting to unblock the release and revisiting.

---

_Comment by @polarathene on 2025-04-30 12:55_

FWIW, on a resource constrained budget VPS (_Fedora 42, 2GB RAM with extra zram swap + 1 vCPU_) builds took notably longer, with a 9MB difference (~20% smaller) between [`fat` and `thin` LTO](https://doc.rust-lang.org/cargo/reference/profiles.html#lto) builds.

Final image (`uv` + `uvx`):
- **40.6MB** - `lto="fat"` => 8,706s (_2 hours 25 minutes_)
- **49.5MB** - `lto="thin"` => 2,598s (_43 minutes_)


---
