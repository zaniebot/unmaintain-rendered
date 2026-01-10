```yaml
number: 3281
title: Run resolve/install benchmarks in ci
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - benchmarks
assignees: []
merged: true
base: main
head: benches
created_at: 2024-04-26T16:41:10Z
updated_at: 2024-04-30T17:39:48Z
url: https://github.com/astral-sh/uv/pull/3281
synced_at: 2026-01-10T14:37:54Z
```

# Run resolve/install benchmarks in ci

---

_Pull request opened by @ibraheemdev on 2024-04-26 16:41_

## Summary

Runs resolver benchmarks in CI with codspeed.

---

_Comment by @codspeed-hq[bot] on 2024-04-26 19:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:benches)


### Congrats! CodSpeed is installed 🎉

`🆕` **12 new benchmarks were detected.**

You will start to see performance impacts in the reports once the benchmarks are run from your default branch.

<details>
  <summary><h3>Detected benchmarks</h3></summary>

- `build_platform_tags[burntsushi-archlinux]` (6.3 ms)
- `wheelname_parsing[flyte-long-compatible]` (21 µs)
- `wheelname_parsing[flyte-long-incompatible]` (26.3 µs)
- `wheelname_parsing[flyte-short-compatible]` (11.9 µs)
- `wheelname_parsing[flyte-short-incompatible]` (12.2 µs)
- `wheelname_parsing_failure[flyte-long-extension]` (2.6 µs)
- `wheelname_parsing_failure[flyte-short-extension]` (2.6 µs)
- `wheelname_tag_compatibility[flyte-long-compatible]` (2.6 µs)
- `wheelname_tag_compatibility[flyte-long-incompatible]` (1.8 µs)
- `wheelname_tag_compatibility[flyte-short-compatible]` (2.5 µs)
- `wheelname_tag_compatibility[flyte-short-incompatible]` (1.1 µs)
- `resolve_warm_jupyter` (366.7 ms)

</details>

---

_@charliermarsh reviewed on 2024-04-26 19:48_

---

_Review comment by @charliermarsh on `crates/bench/benches/uv.rs`:85 on 2024-04-26 19:48_

We can probably use `uv venv` here instead of `virtualenv`, just to remove the external dependency.

---

_@charliermarsh reviewed on 2024-04-26 19:48_

---

_Review comment by @charliermarsh on `crates/bench/benches/uv.rs`:85 on 2024-04-26 19:48_

I think we use `virtualenv` in the benchmark script just because that's designed to benchmark uv against other tools, and so it removes one source of (possible) variance.

---

_Review comment by @ibraheemdev on `crates/bench/benches/uv.rs`:24 on 2024-04-26 20:41_

Should the benchmarks be run on more/different inputs?

---

_@ibraheemdev reviewed on 2024-04-26 20:41_

---

_Marked ready for review by @ibraheemdev on 2024-04-26 20:41_

---

_Comment by @ibraheemdev on 2024-04-26 20:45_

Hmm it doesn't look like the benchmarks are running correctly under Codspeed, the performance report is showing the resolve/install benchmarks running in microseconds.

---

_Comment by @adriencaccia on 2024-04-26 21:03_

Hey @ibraheemdev, I am a co-founder at @CodSpeedHQ!

> Hmm it doesn't look like the benchmarks are running correctly under Codspeed, the performance report is showing the resolve/install benchmarks running in microseconds.

Yes, running arbitrary executables in a benchmark with CodSpeed will not give out relevant results, as most of the compute is done in a new process that is not instrumented.
It would be best to directly call the underlying functions of the library, without relying on the built executable.

For example, calling https://github.com/astral-sh/uv/blob/2af80c28a8e6a2da755ab78f3ea7b028e8b1510c/crates/uv/src/commands/pip_compile.rs#L52 instead of 
https://github.com/ibraheemdev/uv/blob/4ebdc40f60562c05559ac6331abe1a56275e2c8b/crates/bench/benches/uv.rs#L41-L42.

Hope that helps you a bit 😃 

---

_Comment by @ibraheemdev on 2024-04-26 21:06_

@adriencaccia Thanks! I suspected we would have to do this eventually, but didn't realize CodSpeed didn't support processing external commands at all.

---

_Converted to draft by @ibraheemdev on 2024-04-26 21:06_

---

_Label `benchmarks` added by @ibraheemdev on 2024-04-29 18:24_

---

_Label `internal` added by @ibraheemdev on 2024-04-29 18:25_

---

_Marked ready for review by @ibraheemdev on 2024-04-29 20:32_

---

_@charliermarsh approved on 2024-04-30 04:20_

Nice, thank you! Open to giving this a shot. Do we have any sense for what the variance/noise will look like?

---

_Comment by @adriencaccia on 2024-04-30 14:44_

> Nice, thank you! Open to giving this a shot. Do we have any sense for what the variance/noise will look like?

I tested it out on my fork at https://github.com/adriencaccia/uv/pull/1, and I have the following variance results for 101 runs on the same commit:

```text
Found 101 runs for adriencaccia/uv (fca26cde1b54f7467267ca4dff7a9b9cb6f10d29)
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────┬───────────────────┬─────────────────────┬───────────┬──────────────────┐
│                                                                              (index)                                                                               │  average  │ standardDeviation │ varianceCoefficient │   range   │ rangeCoefficient │
├────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┼───────────────────┼─────────────────────┼───────────┼──────────────────┤
│ crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-short-incompatible] │ '1.1 µs'  │     '27.3 ns'     │       '2.5%'        │ '55.6 ns' │      '5.1%'      │
│  crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-short-compatible]  │ '2.5 µs'  │     '27.3 ns'     │       '1.1%'        │ '55.6 ns' │      '2.2%'      │
│  crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-long-compatible]   │ '2.6 µs'  │     '27.3 ns'     │       '1.0%'        │ '55.6 ns' │      '2.1%'      │
│ crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-long-incompatible]  │ '1.8 µs'  │     '13.6 ns'     │       '0.7%'        │ '27.8 ns' │      '1.5%'      │
│    crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing_failure::wheelname_parsing_failure[flyte-short-extension]     │ '2.6 µs'  │     '13.6 ns'     │       '0.5%'        │ '27.8 ns' │      '1.1%'      │
│     crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing_failure::wheelname_parsing_failure[flyte-long-extension]     │ '2.6 µs'  │     '13.6 ns'     │       '0.5%'        │ '27.8 ns' │      '1.1%'      │
│            crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-short-compatible]            │  '12 µs'  │     '13.6 ns'     │       '0.1%'        │ '27.8 ns' │      '0.2%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-short-incompatible]           │ '12.2 µs' │     '13.6 ns'     │       '0.1%'        │ '27.8 ns' │      '0.2%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_build_platform_tags::build_platform_tags[burntsushi-archlinux]           │ '6.3 ms'  │     '13.6 ns'     │       '0.0%'        │ '27.8 ns' │      '0.0%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-long-incompatible]            │ '26.3 µs' │      '0 ns'       │       '0.0%'        │   '0 s'   │      '0.0%'      │
│            crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-long-compatible]             │  '21 µs'  │      '0 ns'       │       '0.0%'        │   '0 s'   │      '0.0%'      │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────┴───────────────────┴─────────────────────┴───────────┴──────────────────┘
```

It is fairly stable, so you should be able to set a low regression threshold to around 5% 🙂 

---

_Comment by @charliermarsh on 2024-04-30 14:51_

Awesome, thanks so much Adrien!

---

_Comment by @zanieb on 2024-04-30 15:06_

Hm I don't see the resolver benchmarks there — I'd expect the distribution filename benches to be very stable but the resolver ones are probably less so.

---

_Comment by @zanieb on 2024-04-30 15:10_

Looks like there's something wrong and the resolver benches are missing on the latest commit.

---

_Comment by @ibraheemdev on 2024-04-30 15:37_

I forgot to use the `codspeed-criterion-compat` shim in the `uv` benchmarks, but it looks like the crate doesn't support async runs.

---

_Comment by @adriencaccia on 2024-04-30 16:14_

@ibraheemdev let me know when you want me to run variance checks again on the new benchmarks

---

_Comment by @ibraheemdev on 2024-04-30 16:16_

@adriencaccia can you run them now? I'm also curious why benchmarks seem to be running ~15x slower on CodSpeed than locally, is it using an aggregate time instead of per-run?

---

_Comment by @adriencaccia on 2024-04-30 16:29_

> @adriencaccia can you run them now? 

Alright, I started them. Will post the results once they are done 😉

> I'm also curious why benchmarks seem to be running ~15x slower on CodSpeed than locally, is it using an aggregate time instead of per-run?

This is because we run the code with `valgrind`, it adds a 4x to 10x overhead, sometimes more. But that is how we get those consistent measures and flamegraphs 😉 

---

_Comment by @adriencaccia on 2024-04-30 16:36_

Results with the new benchmarks:

```text
Found 101 runs for adriencaccia/uv (7bbc18a361ba078e21186db90b98d6b88b3a8a7c)
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬────────────┬───────────────────┬─────────────────────┬───────────┬──────────────────┐
│                                                                              (index)                                                                               │  average   │ standardDeviation │ varianceCoefficient │   range   │ rangeCoefficient │
├────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼────────────┼───────────────────┼─────────────────────┼───────────┼──────────────────┤
│                                               crates/bench/benches/uv.rs::uv::resolve_warm_black::resolve_warm_black                                               │ '15.3 ms'  │    '527.5 µs'     │       '3.4%'        │ '2.3 ms'  │     '15.0%'      │
│ crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-short-incompatible] │  '1.1 µs'  │     '27.5 ns'     │       '2.5%'        │ '55.6 ns' │      '5.1%'      │
│                                             crates/bench/benches/uv.rs::uv::resolve_warm_jupyter::resolve_warm_jupyter                                             │ '366.5 ms' │     '4.2 ms'      │       '1.1%'        │ '22.8 ms' │      '6.2%'      │
│  crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-short-compatible]  │  '2.5 µs'  │     '27.5 ns'     │       '1.1%'        │ '55.6 ns' │      '2.2%'      │
│  crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-long-compatible]   │  '2.6 µs'  │     '27.5 ns'     │       '1.0%'        │ '55.6 ns' │      '2.1%'      │
│ crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_tag_compatibility::wheelname_tag_compatibility[flyte-long-incompatible]  │  '1.9 µs'  │     '13.7 ns'     │       '0.7%'        │ '27.8 ns' │      '1.5%'      │
│    crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing_failure::wheelname_parsing_failure[flyte-short-extension]     │  '2.6 µs'  │     '13.7 ns'     │       '0.5%'        │ '27.8 ns' │      '1.1%'      │
│     crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing_failure::wheelname_parsing_failure[flyte-long-extension]     │  '2.6 µs'  │     '13.7 ns'     │       '0.5%'        │ '27.8 ns' │      '1.1%'      │
│            crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-short-compatible]            │  '12 µs'   │     '13.7 ns'     │       '0.1%'        │ '27.8 ns' │      '0.2%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-short-incompatible]           │ '12.2 µs'  │     '13.7 ns'     │       '0.1%'        │ '27.8 ns' │      '0.2%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_build_platform_tags::build_platform_tags[burntsushi-archlinux]           │  '6.3 ms'  │     '13.7 ns'     │       '0.0%'        │ '27.8 ns' │      '0.0%'      │
│           crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-long-incompatible]            │ '26.3 µs'  │      '0 ns'       │       '0.0%'        │   '0 s'   │      '0.0%'      │
│            crates/bench/benches/distribution_filename.rs::distribution_filename::benchmark_wheelname_parsing::wheelname_parsing[flyte-long-compatible]             │  '21 µs'   │      '0 ns'       │       '0.0%'        │   '0 s'   │      '0.0%'      │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴────────────┴───────────────────┴─────────────────────┴───────────┴──────────────────┘
```

Indeed, it seems that `crates/bench/benches/uv.rs::uv::resolve_warm_black::resolve_warm_black` is a bit more inconsistent

---

_Comment by @charliermarsh on 2024-04-30 16:38_

Maybe we remove the Black test? It seems like the variance is way higher than the Jupyter test.

---

_Comment by @zanieb on 2024-04-30 17:21_

I wonder why that is. It shouldn't be that different? (as far as variance)

---

_Comment by @ibraheemdev on 2024-04-30 17:24_

@zanieb It's probably that the actual resolve step is faster, so the benchmark is more influenced by other factors (file I/O, etc.)

---

_Comment by @ibraheemdev on 2024-04-30 17:39_

I'm going to go ahead and merge this with just the jupyter benchmark. We'll see how consistent/useful the reports are.

---

_Merged by @ibraheemdev on 2024-04-30 17:39_

---

_Closed by @ibraheemdev on 2024-04-30 17:39_

---
