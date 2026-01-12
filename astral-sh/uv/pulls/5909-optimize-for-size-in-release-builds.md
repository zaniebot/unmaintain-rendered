```yaml
number: 5909
title: Optimize for size in release builds
type: pull_request
state: closed
author: davfsa
labels: []
assignees: []
base: main
head: task/optimize-for-size
created_at: 2024-08-08T14:10:11Z
updated_at: 2025-04-28T00:46:42Z
url: https://github.com/astral-sh/uv/pull/5909
synced_at: 2026-01-12T16:07:06Z
```

# Optimize for size in release builds

---

_@davfsa_

## Summary

This is a bit of a followup to #5904, which I did without properly testing this idea, as I expected it to provide way worst results than it seems to have done.

Apart from LTO, I also wanted to test setting `opt-level` to optimize for size (`"s"`). It wasn't the first test I did because I expected LTO to be more of an overall gain, but optimizing for size, not so much. It has still yielded good results in the limited speed testing I have done (which I was afraid it would not do), and wanted to share these here.

The main idea behind these PRs is to slowdown the need for PyPI size increases https://github.com/pypi/support/issues/4260 and overall just being more efficient where possible :)

Marking it as a draft for now to enable discussion (as it seems like this is a hot topic from what I have seen in the ruff repo and #5904).

## Test Plan

<details>

<summary>Expand</summary>

Git commit for each of the executables:
- `uv-main`: https://github.com/astral-sh/uv/commit/acbd367eadc6de2b5accbab1c288076a21190967
- `uv-lto`: https://github.com/astral-sh/uv/commit/cac3c4dfa55df7dbb1a5755a4c8756d6e80d7580
- `uv-size`: https://github.com/astral-sh/uv/commit/50efcde8cac5302a040939b3a2f2e69305d23ddc
- `uv-size-no-lto`: https://github.com/astral-sh/uv/commit/cde0aa1a5034a8e0beb0e9b621208672cd7424d2 with https://github.com/astral-sh/uv/commit/cac3c4dfa55df7dbb1a5755a4c8756d6e80d7580 reverted

System info:
```
CPU: AMD Ryzen 7 3700X (16) @ 3.600GHz
Memory: 32GB @ 3200 MT/s
Uname: Linux galaxy 6.6.44-1-MANJARO #1 SMP PREEMPT_DYNAMIC Sat Aug  3 10:09:33 UTC 2024 x86_64 GNU/Linux
```

---

### Binary Size

Command run: `cargo build --release` and then getting the binaries from `targets/release/uv`

- uv-lto: 25M
- uv-main: 30M
- uv-size-no-lto: 22M
- uv-size: 17M

#### Conclusion
Drastic improvement in size by enabling both LTO and optimizing for size!

### Wheel Size
Command run: `uvx maturin build --release` and then getting the wheels from `targets/wheels/uv-0.2.34-py3-none-linux_x86_64.whl`

- uv-lto: 11M
- uv-main: 12M
- uv-size-no-lto: 8.7M
- uv-size: 7.4M

#### Conclusion
Drastic improvement in size by enabling both LTO and optimizing for wheel size too!

---

### Executable Speed
uv-lto:
```
$ ~/coding/uv/uv-lto run resolver --uv-pip --benchmark resolve-warm --benchmark resolve-cold ../requirements/trio.in
warning: `uv run` is experimental and may change without warning
Benchmark 1: uv pip (resolve-warm)
  Time (mean ± σ):      26.7 ms ±   1.1 ms    [User: 8.5 ms, System: 32.4 ms]
  Range (min … max):    24.6 ms …  31.1 ms    97 runs
 
Benchmark 1: uv pip (resolve-cold)
  Time (mean ± σ):     405.1 ms ±  40.3 ms    [User: 128.7 ms, System: 72.0 ms]
  Range (min … max):   334.3 ms … 473.2 ms    10 runs
```
uv-main:
```
$ ~/coding/uv/uv-main run resolver --uv-pip --benchmark resolve-warm --benchmark resolve-cold ../requirements/trio.in
warning: `uv run` is experimental and may change without warning
Benchmark 1: uv pip (resolve-warm)
  Time (mean ± σ):      26.6 ms ±   1.1 ms    [User: 8.5 ms, System: 32.5 ms]
  Range (min … max):    23.6 ms …  30.4 ms    98 runs
 
Benchmark 1: uv pip (resolve-cold)
  Time (mean ± σ):     366.9 ms ±  36.0 ms    [User: 115.9 ms, System: 77.1 ms]
  Range (min … max):   325.4 ms … 435.9 ms    10 runs
```
uv-size-no-lto:
```
$ ~/coding/uv/uv-size-no-lto run resolver --uv-pip --benchmark resolve-warm --benchmark resolve-cold ../requirements/trio.in
warning: `uv run` is experimental and may change without warning
Benchmark 1: uv pip (resolve-warm)
  Time (mean ± σ):      27.0 ms ±   1.1 ms    [User: 9.0 ms, System: 33.0 ms]
  Range (min … max):    23.3 ms …  29.5 ms    91 runs
 
Benchmark 1: uv pip (resolve-cold)
  Time (mean ± σ):     373.8 ms ±  37.7 ms    [User: 103.2 ms, System: 77.5 ms]
  Range (min … max):   328.8 ms … 424.3 ms    10 runs
```
uv-size:
```
$ ~/coding/uv/uv-size run resolver --uv-pip --benchmark resolve-warm --benchmark resolve-cold ../requirements/trio.in
warning: `uv run` is experimental and may change without warning
Benchmark 1: uv pip (resolve-warm)
  Time (mean ± σ):      26.5 ms ±   0.9 ms    [User: 9.4 ms, System: 31.7 ms]
  Range (min … max):    24.5 ms …  28.8 ms    104 runs
 
Benchmark 1: uv pip (resolve-cold)
  Time (mean ± σ):     377.8 ms ±  24.5 ms    [User: 113.7 ms, System: 74.5 ms]
  Range (min … max):   346.7 ms … 406.1 ms    10 runs
```

#### Result
I am not entirely sure why `uv-lto` is slighly slower than `uv-main` even tho benchmarks say otherwise (re-ran the benchmark multiple times), but will leave it to codespeed to determine that by properly running the benchmarks. What I was aiming to prove with these benchmarks is that optimizing for speed doesn't actually slow down uv that much, mostly in what I would call an "acceptable" manner, considering how fast uv is already.

---

### Build times
Massive thanks to @T-256 for bringing up https://github.com/astral-sh/ruff/issues/9224. After reading through it and its sub-links, this [comment by zanieb](https://github.com/astral-sh/ruff/issues/9224#issuecomment-1866415345) really made me realize that maybe ensuring build times are not too drastic is something worth ensuring, as to not make it harder for developers getting into the needy giddy of optimizing (and as I have also found out, lol)

Numbers are taken directly from the "finished" message using `cargo build --release`.

**They are not real benchmarks, they are single runs, and must be taken with a grain of salt, but give good indications of what is going on**

#### Cold run (after `cargo clean`)

- uv-lto: 3m 41s
- uv-main: 1m 32s
- uv-size-no-lto: 1m 03s
- uv-size: 2m 14s

Conclusions: Slight improvement in build times compared to LTO builds, but still half a minute slower than what it was before enabling LTO

#### Warm run

*Didn't really have any good ideas on what a warm run would look like, so I havent run these. If anybody knows a good way to do this, please do let me know and I will run them*

</details>

---

_Converted to draft by @davfsa on 2024-08-08 14:10_

---

_Comment by @bluss on 2024-08-08 14:11_

Is lto = "thin" an option by the way, where does it land w.r.t time and size?

---

_Comment by @davfsa on 2024-08-08 14:15_

> Is lto = "thin" an option by the way, where does it land w.r.t time and size?

Didn't test it, only now realized that `profile.dist` (for `cargo dist` builds) seems to run with it. Will post the results when I can!

---

_Comment by @codspeed-hq[bot] on 2024-08-08 14:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/davfsa:task/optimize-for-size)

### Merging #5909 will **improve performances by 15.12%**

<sub>Comparing <code>davfsa:task/optimize-for-size</code> (54d259b) with <code>main</code> (2822dde)</sub>



### Summary

`⚡ 2` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `davfsa:task/optimize-for-size` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-long-compatible]` | 2 µs | 1.8 µs | +11.47% |
| ⚡ | `wheelname_tag_compatibility[flyte-short-compatible]` | 2 µs | 1.7 µs | +15.12% |


---

_Comment by @davfsa on 2024-08-08 14:25_

@bluss Didn't fully test the speed of using `lto = "thin"`, but just after a normal build and seeing the size of the executable size, I dont think it would be much better performance wise nor even worth testing:

Binary size: 31MB
Wheel size (py3-none-linux_x86_64):  13MB

---

_Comment by @BurntSushi on 2024-08-08 14:33_

I would be somewhat skeptical of using `z` because of its possible impacts on runtime perf. IIRC, it disables auto-vectorization.

IMO, if we're looking to decrease binary size, we should be chasing other avenues first. Like changes to the code itself that decrease bloat. We have barely done any of that, and I expect there are probably a lot of low hanging fruits.

---

_Comment by @davfsa on 2024-08-08 14:37_

> I would be somewhat skeptical of using z because of its possible impacts on runtime perf. IIRC, it disables auto-vectorization.

Quick look at https://doc.rust-lang.org/cargo/reference/profiles.html#opt-level does mention that it will also disable loop vectorization. I will try `"s"` instead and see the size/performance hits (my mind instantly went to "z", because it is what I have used in other projects)

> IMO, if we're looking to decrease binary size, we should be chasing other avenues first. Like changes to the code itself that decrease bloat. We have barely done any of that, and I expect there are probably a lot of low hanging fruits.

I am not too familiar with UVs code, but if there is anywhere you think I should be looking in particular, let me know and will explore in that direction!


---

_Comment by @BurntSushi on 2024-08-08 14:39_

> I am not too familiar with UVs code, but if there is anywhere you think I should be looking in particular, let me know and will explore in that direction!

I would look for uses of generics and either remove them, limit the scope of their monomorphization or switch to `dyn` if possible. And I would start near the root of the crate. The highest value functions to attack are the biggest ones.

---

_Comment by @BurntSushi on 2024-08-08 14:40_

The other dimension to look at are dependencies. We are not especially conservative with bringing in new dependencies. I wouldn't be surprised if there were some we could pluck out, either with a little extra code on our part or just different feature configurations.

---

_Comment by @BurntSushi on 2024-08-08 14:41_

`cargo bloat` can probably help identify the biggest functions. And it should also point out when there are multiple copies of the same function.

---

_Comment by @davfsa on 2024-08-08 14:44_

> I will try "s" instead and see the size/performance hits

It seems to be mostly on par to using `z` (+1MB binary size only, which I think its acceptable). Will update this PR and the description.

---

Thanks @BurntSushi for all the helpful information, will look into it!

---

_Comment by @davfsa on 2024-08-08 15:02_

Ok, really confused right now, but benchmarks now report a performance increase? Huh?

Thats unexpected :thinking: 

---

_Comment by @davfsa on 2024-08-08 17:27_

Did some reading on the flags, and it seems like optimizing for speed (levels 2 and 3) only really do more aggressive inlining and vectorization, as well as loop unrolling. Otherwise, optimizing for space has the same other optimizations as enabling optimizations (except for the ones outlined before).

So I guess its a good thing to enable by default and it makes sense that the benchmarks do not report any slowdowns (the speedups are interesting tho).

Think this is fine to set a ready for merge!

reference: https://docs.rust-embedded.org/book/unsorted/speed-vs-size.html#optimize-for-speed

---

On another note, the point raised by BurntSushi regarding build times i found to be quite important, so might have a look tomorrow and open another pr/issue if i find anything interesting to share :)

---

_Marked ready for review by @davfsa on 2024-08-08 17:27_

---

_Comment by @BurntSushi on 2024-08-08 17:31_

Our CI benchmarks don't have the best coverage at the moment. We'll want to run some of the hyperfine benchmarks locally to test the effects of this. My prior here is that we should have our `opt-level = 3`. We _want_ aggressive inlining and vectorization.

---

_Comment by @davfsa on 2024-08-08 17:41_

> Our CI benchmarks don't have the best coverage at the moment. We'll want to run some of the hyperfine benchmarks locally to test the effects of this.

Would be happy to do so. The benchmarks I attached to the description of the issue seem to indicate no drastic slowdowns, but if you indicate which benchmarks I should perform, I'll perform them and post my findings here.

> My prior here is that we should have our opt-level = 3. We want aggressive inlining and vectorization.

Vectorization will be just like `opt-level = 3`, it was `opt-level = z` that was removing it (which i assume caused the slowdown the bench marks first reported). I could play around with `inline-threshold` and see how close I can get it to the `opt-level = 2` and `opt-level = 3` values of 225 and 275 respectively.

Will report back!

---

_Comment by @davfsa on 2024-08-08 18:27_

Would like to add this information to the discussion: it seems that in rustc 1.18.0+, using `opt-level = s` is a combination of LLVM flags, setting speed optimization to 2 (ending in the same optimizations as `opt-level = 2`) and size optimization to 1 (2 being aggressive).

From what I can gather, optimizing for space with "s" is outputting an executable that is almost 2x smaller than the previous ones, while still staying mostly performant as previously.

I would be interested in running proper benchmarks to actually see what the performance cost would be going from O3 + LTO to O2 + size + LTO (and playing around with flags to see what gives the best results)

reference: https://github.com/rust-lang/rust/blob/d3a393932eeafa4638ae22f5ecbc38bf38760d0e/compiler/rustc_codegen_ssa/src/base.rs#L1016-L1018

---

_Comment by @samypr100 on 2024-08-08 23:56_

https://github.com/johnthagen/min-sized-rust is also a often a recommended reference, but I agree with @BurntSushi that there might be other areas to look at first

---

_Comment by @charliermarsh on 2025-04-28 00:46_

I'm going to close this for now since we're not actively exploring it.

---

_Closed by @charliermarsh on 2025-04-28 00:46_

---
