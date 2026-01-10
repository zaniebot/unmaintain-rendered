```yaml
number: 9031
title: "release: switch to Cargo's default"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/default-release-experiment
created_at: 2023-12-07T00:37:05Z
updated_at: 2023-12-15T13:19:36Z
url: https://github.com/astral-sh/ruff/pull/9031
synced_at: 2026-01-10T23:31:11Z
```

# release: switch to Cargo's default

---

_Pull request opened by @BurntSushi on 2023-12-07 00:37_

This sets `lto = "thin"` instead of using "fat" LTO, and sets `codegen-units = 16`. These are the defaults for Cargo's `release` profile, and I think it may give us faster iteration times, especially when benchmarking. The point of this PR is to see what kind of impact this has on benchmarks. It is expected that benchmarks may regress to some extent.

I did some quick ad hoc experiments to quantify this change in compile times. Namely, I ran:

    cargo build --profile release -p ruff_cli

Then I ran

    touch crates/ruff_python_formatter/src/expression/string/docstring.rs

(because that's where i've been working lately) and re-ran

    cargo build --profile release -p ruff_cli

This last command is what I timed, since it reflects how much time one has to wait between making a change and getting a compiled artifact.

Here are my results:

* With status quo `release` profile, build takes 77s
* with `release` but `lto = "thin"`, build takes 41s
* with `release`, but `lto = false`, build takes 19s
* with `release`, but `lto = false` **and** `codegen-units = 16`, build takes 7s
* with `release`, but `lto = "thin"` **and** `codegen-units = 16`, build takes 16s (i believe this is the default `release` configuration)

This PR represents the last option. It's not the fastest to compile, but it's nearly a whole minute faster! The idea is that with `codegen-units = 16`, we still make use of parallelism, but keep _some_ level of LTO on to try and re-gain what we lose by increasing the number of codegen units.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-07 00:37_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-07 00:37_

---

_Review requested from @zanieb by @BurntSushi on 2023-12-07 00:37_

---

_Review requested from @konstin by @BurntSushi on 2023-12-07 00:37_

---

_Comment by @charliermarsh on 2023-12-07 01:36_

Looks like the benchmarks didn't run 🤔 

---

_Comment by @MichaReiser on 2023-12-07 01:55_

Yeah, seems like our determine changes is too aggressive. 

Would you mind running our hyperfine benchmarks (linting the cpython code base) in addition to the micro benchmarks to get a better understanding of how the performance of the CLI etc is impacted?

---

_Label `internal` added by @MichaReiser on 2023-12-07 01:55_

---

_Comment by @zanieb on 2023-12-07 02:40_

Hm looks like #8225 has a bug in it since this _should_ have been detected as a code change @Cjkjvfnby

---

_Comment by @zanieb on 2023-12-07 02:53_

Here's the fix? ~https://github.com/astral-sh/ruff/pull/9035~ https://github.com/astral-sh/ruff/pull/9038

---

_@konstin approved on 2023-12-07 13:07_

While we're at it, could we rename `release-debug` to `profiling`? That makes it clearer why this profile exists.

---

_Comment by @codspeed-hq[bot] on 2023-12-07 13:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ag/default-release-experiment)

### Merging #9031 will **degrade performances by 5.92%**

<sub>Comparing <code>ag/default-release-experiment</code> (9bff2ec) with <code>main</code> (c014622)</sub>



### Summary

`❌ 7` regressions
`✅ 23` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ag/default-release-experiment)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ag/default-release-experiment` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.2 ms | -4.38% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 73 ms | 77.6 ms | -5.92% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.7 ms | 36.2 ms | -4.3% |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 4.2 ms | 4.5 ms | -5.02% |
| ❌ | `linter/all-with-preview-rules[pydantic/types.py]` | 81.1 ms | 85 ms | -4.59% |
| ❌ | `linter/all-with-preview-rules[large/dataset.py]` | 187.2 ms | 198.7 ms | -5.77% |
| ❌ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 37.3 ms | 39.6 ms | -5.79% |


---

_Comment by @github-actions[bot] on 2023-12-07 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @konstin on 2023-12-07 14:17_

Given the regressions, fwiw i'm also fine with different settings for `profiling` and `release`

---

_Comment by @BurntSushi on 2023-12-07 15:00_

OK, so I followed @charliermarsh's suggestion to run a `hyperfine` benchmark on CPython. To do that, I created profiles for each configuration we want to test (basically `{fatlto, thinlto, nolto} x {cg=1, cg=16}`):

```toml
[profile.fatcg1]
inherits = "release"
lto = "fat"
codegen-units = 1

[profile.fatcg16]
inherits = "release"
lto = "fat"
codegen-units = 16

[profile.thincg1]
inherits = "release"
lto = "thin"
codegen-units = 1

[profile.thincg16]
inherits = "release"
lto = "thin"
codegen-units = 16

[profile.noltocg1]
inherits = "release"
lto = false
codegen-units = 1

[profile.noltocg16]
inherits = "release"
lto = false
codegen-units = 16
```

Then I compiled a binary for each profile:

```bash
cargo clean
mkdir -p target/release
for p in fatcg1 fatcg16 thincg1 thincg16 noltocg1 noltocg16; do
  cargo build --profile $p -p ruff_cli
  cp target/$p/ruff target/release/ruff-$p
done
```

And baked them off:

```
hyperfine \
    --warmup 10 \
    --runs 100 \
    "ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e" \
    "ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e" \
    "ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e" \
    "ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e" \
    "ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e" \
    "ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e"
Benchmark 1: ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     125.8 ms ±   3.6 ms    [User: 2001.5 ms, System: 137.6 ms]
  Range (min … max):   120.0 ms … 143.9 ms    100 runs

Benchmark 2: ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     126.9 ms ±   4.3 ms    [User: 1991.5 ms, System: 142.3 ms]
  Range (min … max):   119.1 ms … 140.6 ms    100 runs

Benchmark 3: ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     126.0 ms ±   4.1 ms    [User: 1991.0 ms, System: 138.9 ms]
  Range (min … max):   119.1 ms … 137.8 ms    100 runs

Benchmark 4: ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     127.9 ms ±   4.1 ms    [User: 2016.1 ms, System: 133.5 ms]
  Range (min … max):   120.5 ms … 139.8 ms    100 runs

Benchmark 5: ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     132.8 ms ±   3.7 ms    [User: 2135.2 ms, System: 126.4 ms]
  Range (min … max):   126.7 ms … 141.6 ms    100 runs

Benchmark 6: ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     132.7 ms ±   4.4 ms    [User: 2115.1 ms, System: 133.7 ms]
  Range (min … max):   125.3 ms … 148.1 ms    100 runs

Summary
  ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e ran
    1.00 ± 0.04 times faster than ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
    1.01 ± 0.04 times faster than ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
    1.02 ± 0.04 times faster than ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
    1.05 ± 0.05 times faster than ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
    1.06 ± 0.04 times faster than ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e
```

As expected, `ruff-fatcg1` is the fastest, but `ruff-thincg1`, `ruff-fatcg16` and `ruff-thincg16` are all extremely close. (This PR has the `thincg16` configuration.)

So the microbenchmark regressions here do indeed look a little scary, but the more holistic/realistic benchmark looks okay to me?

---

_Comment by @BurntSushi on 2023-12-07 15:10_

> fwiw i'm also fine with different settings for `profiling` and `release`

In theory I'm fine with it too, but I do feel like it can be pretty tricky. What I'm thinking about is something like this:

1. You submit a PR.
2. codspeed benchmarks run and show a small but measurable regression.
3. You run benchmarks and profile things with the `profiling` profile.

In this case, you might be seeing something very different than what was benchmarked in the PR, and tracking down the regression could prove quite annoying. LTO can greatly impact function inlining. My suspicion is that, in most cases, if a regression exists with LTO enabled, then it probably also exists with LTO disabled (or in a different mode). But not necessarily.

With that said, yeah, if we find we can't relax the LTO configuration then given the difference in compile times here, I'd probably accept the above as a downside I'd be willing to pay I think?

---

_Comment by @charliermarsh on 2023-12-07 15:13_

@BurntSushi - Would it be easy to re-run that comparison with `--select ALL`?

---

_Comment by @dhruvmanila on 2023-12-07 15:14_

Just a note that we're moving ahead on https://github.com/astral-sh/ruff/pull/8835 with the regression. I'm not sure if this would affect the code generation problem in that PR anyhow but we should take the combined regression into account.

---

_@dhruvmanila reviewed on 2023-12-07 15:17_

---

_Review comment by @dhruvmanila on `Cargo.toml`:120 on 2023-12-07 15:17_

We would also want to update all references of `release-debug` in the profiling section of [`CONTRIBUTING.md`](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#benchmarking-and-profiling).

---

_Comment by @BurntSushi on 2023-12-07 15:47_

> @BurntSushi - Would it be easy to re-run that comparison with `--select ALL`?

@charliermarsh Yeah! Here you go:

```
hyperfine \
    --warmup 10 \
    --runs 100 \
    "ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL" \
    "ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL" \
    "ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL" \
    "ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL" \
    "ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL" \
    "ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL"
Benchmark 1: ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     414.5 ms ±   8.0 ms    [User: 5297.2 ms, System: 329.0 ms]
  Range (min … max):   397.6 ms … 453.5 ms    100 runs

Benchmark 2: ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     413.8 ms ±   7.2 ms    [User: 5257.4 ms, System: 329.6 ms]
  Range (min … max):   399.5 ms … 436.6 ms    100 runs

Benchmark 3: ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     433.8 ms ±   7.5 ms    [User: 5352.2 ms, System: 325.2 ms]
  Range (min … max):   416.0 ms … 453.6 ms    100 runs

Benchmark 4: ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     435.4 ms ±   9.1 ms    [User: 5491.0 ms, System: 329.1 ms]
  Range (min … max):   412.2 ms … 461.6 ms    100 runs

Benchmark 5: ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     470.0 ms ±  11.1 ms    [User: 5718.5 ms, System: 319.6 ms]
  Range (min … max):   446.5 ms … 498.2 ms    100 runs

Benchmark 6: ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
  Time (mean ± σ):     472.9 ms ±  10.5 ms    [User: 5772.5 ms, System: 324.8 ms]
  Range (min … max):   451.5 ms … 501.0 ms    100 runs

Summary
  ruff-fatcg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL ran
    1.00 ± 0.03 times faster than ruff-fatcg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
    1.05 ± 0.03 times faster than ruff-thincg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
    1.05 ± 0.03 times faster than ruff-thincg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
    1.14 ± 0.03 times faster than ruff-noltocg1 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
    1.14 ± 0.03 times faster than ruff-noltocg16 ./crates/ruff_linter/resources/test/cpython/ --no-cache --silent -e --select ALL
```

So in this case, we end up with a 1.05x regression for both `thincg1` and `thincg16`. Interestingly, `fatcg16` seems about on par with `fatcg1`. I thought maybe we could get a free win by switching to `fatcg16`, but compile times are actually worse in that configuration. Using my test outlined in the initial comment in this PR, the re-build time is 92s (versus 77s for `fatcg1`). I speculate that the compile times are worse because the `codegen-units = 16` ends up creating more work for fat LTO.

---

_Comment by @MichaReiser on 2023-12-08 00:15_

Sorry to ask for more benchmarks, but it would be good to have some numbers on the formatter too:

```bash
hyperfine ./target/release/ruff format ./checkouts/zulip
```

And you may want to benchmark another project than CPython (one that actually uses ruff like homeassistant or `zulip`) for `--select ALL` because CPython has so many violations that you manly profile the diagnostic printing and caching of a vast amount if diagnostics (atypical workload)

I'm a bit surprised that the lexer microbenchmarks are affected that much... Could it be that Rust directly inlines too much of the lexer into the benchmark, removing multiple function calls? I otherwise wouldn't expect much change because the lexer code is mostly self contained in one crate (and called by the parser from the same crate). 

It may be worth to compare the profiles between LTO1 and THIN16 to see if there are some obvious candidates where it makes sense to add an `#[inline]` attribute to preserve the cross-crate inlining that the linker did automatically with fat but doesn't with thin. For example, adding inline to `Cursor::eat_while` is a 4% perf improvement on my machine. 

It would be nice if we could specify the code units per crate. E.g. `ruff_python_ast` and parser change infrequently but a fast parser is important. Having `code_units=1` might be worth it for the parser without reducing your iteration speed much.

> In theory I'm fine with it too, but I do feel like it can be pretty tricky. What I'm thinking about is something like this:

I think I'm otherwise okay with a 3-5% regression, if we have sufficient proof that it boosts our productivity significantly (Note: This won't improve our CI times other than for the benchmarks run)

---

_Comment by @BurntSushi on 2023-12-13 19:09_

All righty, I re-ran `ruff` on the `zulip` repo with `--select ALL`:

```
$ hyperfine \
    --warmup 10 \
    --runs 100 \
    "ruff-fatcg1 ./ --no-cache --silent -e --select ALL" \
    "ruff-fatcg16 ./ --no-cache --silent -e --select ALL" \
    "ruff-thincg1 ./ --no-cache --silent -e --select ALL" \
    "ruff-thincg16 ./ --no-cache --silent -e --select ALL" \
    "ruff-noltocg1 ./ --no-cache --silent -e --select ALL" \
    "ruff-noltocg16 ./ --no-cache --silent -e --select ALL"
Benchmark 1: ruff-fatcg1 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     127.2 ms ±   5.8 ms    [User: 1567.6 ms, System: 114.5 ms]
  Range (min … max):   114.0 ms … 141.1 ms    100 runs

Benchmark 2: ruff-fatcg16 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     128.7 ms ±   5.6 ms    [User: 1571.7 ms, System: 111.6 ms]
  Range (min … max):   115.9 ms … 141.8 ms    100 runs

Benchmark 3: ruff-thincg1 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     131.6 ms ±   5.2 ms    [User: 1605.3 ms, System: 111.5 ms]
  Range (min … max):   121.1 ms … 145.8 ms    100 runs

Benchmark 4: ruff-thincg16 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     133.8 ms ±   6.3 ms    [User: 1646.6 ms, System: 118.6 ms]
  Range (min … max):   121.3 ms … 151.4 ms    100 runs

Benchmark 5: ruff-noltocg1 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     142.3 ms ±   6.4 ms    [User: 1733.6 ms, System: 116.4 ms]
  Range (min … max):   128.9 ms … 160.0 ms    100 runs

Benchmark 6: ruff-noltocg16 ./ --no-cache --silent -e --select ALL
  Time (mean ± σ):     143.6 ms ±   5.8 ms    [User: 1774.2 ms, System: 114.8 ms]
  Range (min … max):   130.7 ms … 159.8 ms    100 runs

Summary
  ruff-fatcg1 ./ --no-cache --silent -e --select ALL ran
    1.01 ± 0.06 times faster than ruff-fatcg16 ./ --no-cache --silent -e --select ALL
    1.03 ± 0.06 times faster than ruff-thincg1 ./ --no-cache --silent -e --select ALL
    1.05 ± 0.07 times faster than ruff-thincg16 ./ --no-cache --silent -e --select ALL
    1.12 ± 0.07 times faster than ruff-noltocg1 ./ --no-cache --silent -e --select ALL
    1.13 ± 0.07 times faster than ruff-noltocg16 ./ --no-cache --silent -e --select ALL
```

And I also checked `ruff format` on the Zulip repo too (being careful to reset the changes between each run, so that we actually test the time it takes to format the code):

```
$ hyperfine \
    --warmup 10 \
    --prepare 'git reset --hard main' \
    --cleanup 'git reset --hard main' \
    'ruff-fatcg1 format ./' \
    'ruff-fatcg16 format ./' \
    'ruff-thincg1 format ./' \
    'ruff-thincg16 format ./' \
    'ruff-noltocg1 format ./' \
    'ruff-noltocg16 format ./'
Benchmark 1: ruff-fatcg1 format ./
  Time (mean ± σ):      47.8 ms ±   2.4 ms    [User: 126.9 ms, System: 57.7 ms]
  Range (min … max):    41.9 ms …  51.5 ms    41 runs

Benchmark 2: ruff-fatcg16 format ./
  Time (mean ± σ):      49.8 ms ±   2.0 ms    [User: 131.2 ms, System: 57.4 ms]
  Range (min … max):    45.1 ms …  53.9 ms    43 runs

Benchmark 3: ruff-thincg1 format ./
  Time (mean ± σ):      49.2 ms ±   1.8 ms    [User: 132.1 ms, System: 58.9 ms]
  Range (min … max):    44.9 ms …  53.5 ms    43 runs

Benchmark 4: ruff-thincg16 format ./
  Time (mean ± σ):      50.0 ms ±   2.1 ms    [User: 131.6 ms, System: 59.4 ms]
  Range (min … max):    45.6 ms …  53.9 ms    43 runs

Benchmark 5: ruff-noltocg1 format ./
  Time (mean ± σ):      52.6 ms ±   2.0 ms    [User: 148.2 ms, System: 56.9 ms]
  Range (min … max):    47.2 ms …  56.6 ms    42 runs

Benchmark 6: ruff-noltocg16 format ./
  Time (mean ± σ):      52.1 ms ±   2.0 ms    [User: 145.1 ms, System: 57.3 ms]
  Range (min … max):    47.3 ms …  58.0 ms    40 runs

Summary
  ruff-fatcg1 format ./ ran
    1.03 ± 0.06 times faster than ruff-thincg1 format ./
    1.04 ± 0.07 times faster than ruff-fatcg16 format ./
    1.05 ± 0.07 times faster than ruff-thincg16 format ./
    1.09 ± 0.07 times faster than ruff-noltocg16 format ./
    1.10 ± 0.07 times faster than ruff-noltocg1 format ./
```

It looks like the relative difference here is about the same as with linting.

My target or hope here is to switch to `thincg16` (the default Cargo `release` profile), since I think it strikes a good balance and shaves an entire minute off our current 77s release build times. I do kind of feel like a 1.05x regression is acceptable given the enormous savings in build time. But let's do some more digging first. I think @MichaReiser has a good idea here that we might actually be able to recuperate our losses with some well-placed `inline` annotations.

I was indeed able to add an `#[inline]` annotation to `Cursor::eat_while` and that seems to help the lexer microbenchmark a little bit.

I looked for other opportunities like that but came up short. I did see some functions inlined with `fatcg1` but not `thincg16`. For example, `alloc::raw_vec::RawVec<T,A>::allocate_in`. But I don't know how to force a function from inside of `std` to get inlined unfortunately. And nothing else really jumps out at me.

> I think I'm otherwise okay with a 3-5% regression, if we have sufficient proof that it boosts our productivity significantly (Note: This won't improve our CI times other than for the benchmarks run)

Aye. It's hard to provide proof, but I do actually feel somewhat strongly that this kind of improvement in iteration time will eventually pay for itself. Ideally, we could get it even faster than 20 seconds, but I think disabling thin LTO is probably a bridge too far. Getting better iteration times after this I think will involve figuring out how to make `ruff` compile faster in other ways.

> It would be nice if we could specify the code units per crate. E.g. ruff_python_ast and parser change infrequently but a fast parser is important. Having code_units=1 might be worth it for the parser without reducing your iteration speed much.

Oh hey! Actually this can be done! I had assumed `codegen-units` was a global setting (like `lto` is), but it's not. I added these lines to `Cargo.toml`:

```toml
# Some crates don't change as much but benefit more from
# more expensive optimization passes, so we selectively
# decrease codegen-units in some cases.
[profile.release.package.ruff_python_parser]
codegen-units = 1
[profile.release.package.ruff_python_ast]
codegen-units = 1
```

And at least locally, this seems to shrink the regression for the lexer microbenchmark substantially. I just pushed up that change here, so let's see how it does with codspeed.

I also re-ran the hyperfine benchmarks above with this new configuration, but it doesn't detect any difference between it and `thincg16`.

---

_Comment by @BurntSushi on 2023-12-13 19:22_

The micro-benchmark regressions are now quite a bit smaller: https://github.com/astral-sh/ruff/pull/9031/checks?check_run_id=19612967302

Summary: This PR reverts the release profile to the default configuration, but does set `codegen-units = 1` for `ruff_python_parser` and `ruff_python_ast`. Overall, this appears to result in about a 1.05x regression on real world workloads, in addition to several micro-benchmark regressions in the 4-6% range. In exchange, iterative builds drop from about 77s to 17s (on my machine), which is IMO very substantial. The key benefit I think this has is that it improves iteration times. I think that faster iteration times _encourage_ more iteration and have less of a chance of disrupting flow states. So overall, I feel like this change is worth making despite the small regression in perf.

---

_@MichaReiser approved on 2023-12-14 03:04_

Thanks for pushing for this and the detailed profiling. This seems a worthwhile trade off to me which most users shouldn't even notice. Long compile times have been a real concern for many contributors and can be very noticable if you work on a somewhat older computer. 

I'm all in for merging as is. We can reconsider using lto=fat in the future if we managed to improve our crate structure, with future rust versions or when having better devtooling (earthly with incremental builds?)

---

_Comment by @T-256 on 2023-12-14 07:28_

Is this regression also effects on final published releases?
If yes, then imo we need consider use separated release profile. (e.g. optimized or publish)
As final user, the speed important for us, as far as publishing happens on few weeks so I don't think compile-time matters.

I mean `profile.release` be compile fast but slower runtime, and `profile.publish` be most optimized on runtime but compile slower.
btw this PR seems fine to me, we can track new profile on new PR :)

---

_Comment by @BurntSushi on 2023-12-14 13:49_

@T-256 I discussed that point [here](https://github.com/astral-sh/ruff/pull/9031#issuecomment-1845520046). The core issue is that if the thing you're benchmarking and the thing you release are different, then you risk measuring and tuning the wrong thing. I don't mean to say that this is an iron-clad argument against something like having a `publish` profile that is only used to cut a release, but rather, it feels like a cost to me. I'm not strongly opposed to a setup like that. It's also very hard to say how big of a cost it is.

---

_@charliermarsh reviewed on 2023-12-14 18:39_

This is very good and thorough work. I'm _slightly_ less convinced that this change is worth the tradeoffs (as compared to @MichaReiser) since it only helps with release builds, and we do release builds locally so infrequently compare to how often Ruff is run in the wild by users. But the size of the regression is not so great that I would block it if you two are in favor.

---

_Comment by @MichaReiser on 2023-12-15 01:02_

> This is very good and thorough work. I'm _slightly_ less convinced that this change is worth the tradeoffs (as compared to @MichaReiser) since it only helps with release builds, and we do release builds locally so infrequently compare to how often Ruff is run in the wild by users. But the size of the regression is not so great that I would block it if you two are in favor.

I agree that must of us don't to frequent release builds (at least for publishing), but they're ferquently needed when doing performance work where building the benchmarks and running our benchmarks takes a significant amount of time (to be fair, running them probably takes longer than building). Having a faster feedback-cycle there would certainly help. 

An alternative is to use different profiles between releasing and profiling but it comes with the downside that what we see in profiles might not match what we see in production. But maybe that's something we have to accept anyway if we e.g. consider using data driven optimiations (run ruff with a typical workload and then feed that information into the optimizer, similar to what Rust does). 

Yet another alternative is to have a light and "full" profiling profile where you can do both a full and light profiling, compare if you see the same outliers, and then use the "light" profilie to optimize the code until the outlier is gone.

---

_Comment by @T-256 on 2023-12-15 01:37_

> An alternative is to use different profiles between releasing and profiling but it comes with the downside that what we see in profiles might not match what we see in production

Production builds intended to be always faster at run-time (?) We can document build profiles: "For final production builds, we are using optimized compilation flags to have most optimized run-time."

For measuring, we can always rely on default release profile, since always comparing to same builds then I think we'd have correct comparation for new changes.

When need to compare against foreign tools (e.g. promoting Ruff against tools/competitors) we can use production build measured numbers.

For now, I think we can merge current PR. production builds needs separated discussion.

---

_Comment by @BurntSushi on 2023-12-15 13:19_

I do find myself more sympathetic to the idea of having different profile settings for typical benchmarking and the final release build. But, let's see how far we can get with having them use the same configuration. We can always revisit this and at least change the `release` profile back to fat LTO and `codegen-units = 1` while simultaneously changing the `profiling` profile to thin LTO and `codegen-units = 16`.

---

_Merged by @BurntSushi on 2023-12-15 13:19_

---

_Closed by @BurntSushi on 2023-12-15 13:19_

---

_Branch deleted on 2023-12-15 13:19_

---
