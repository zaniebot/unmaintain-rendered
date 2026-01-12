```yaml
number: 1225
title: Speedup ripgrep using profile guided optimization
type: issue
state: closed
author: ghost
labels:
  - question
assignees: []
created_at: 2019-03-22T20:22:09Z
updated_at: 2023-11-28T02:34:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1225
synced_at: 2026-01-12T16:13:23Z
```

# Speedup ripgrep using profile guided optimization

---

_@ghost_

### Request for improvement

#### What version of ripgrep are you using?

compiled current master (09139721047b1cda6ad88dbf89dc5fa74c66a3a2)

#### How did you install ripgrep?

compiled current master (09139721047b1cda6ad88dbf89dc5fa74c66a3a2)

#### What operating system are you using ripgrep on?

Linux Mint 19.1 with kernel 4.18.0-16-lowlatency

#### Describe your question, feature request, or bug.

Profile guided optimization works by building the binary with profiling instrumentation first, then running the instrumented binary in a few test cases while saving runtime profiles and then using the profiles to optimize the code for example by reordering functions to improve cachability.

Using the very naive benchmark `time sudo target/release/rg --with-filename --word-regexp --line-buffered a /home > /dev/null` ripgrep needed `3.7s` on avg without profile guided optimization on my system and `3.3s` with pgo.

This is how I compiled rg with pgo:
````
# make sure we get a profile for each run, %p will be replaced with the pid
export LLVM_PROFILE_FILE=./target/pgo/pgo-%p.profraw

# compile instrumented binary
RUSTFLAGS="-Z pgo-gen=llvm-profile-file-env-variable-overrides-this" cargo +nightly build --release

# run a few test cases (these need to be improved to cover more rg features)
target/release/rg --help > /dev/null
target/release/rg a > /dev/null
target/release/rg B --line-buffered > /dev/null
target/release/rg c --word-regexp > /dev/null
target/release/rg D --vimgrep > /dev/null
target/release/rg e --with-filename > /dev/null
target/release/rg f --unrestricted > /dev/null
target/release/rg '[A-Z]+_SUSPEND' --with-filename --word-regexp --line-buffered > /dev/null
target/release/rg h --unrestricted --with-filename --vimgrep --word-regexp --line-buffered > /dev/null

# merge profiles
rustup run nightly llvm-profdata merge -o target/pgo/pgo.profdata target/pgo/pgo*.profraw

# compile with profile in mind
RUSTFLAGS="-Z pgo-use=target/pgo/pgo.profdata" cargo +nightly build --release
````

pgo could speed up ripgrep quite a bit when done correctly.

---

_Comment by @BurntSushi on 2019-03-22 21:37_

Neat. Could you please document the build dependencies here more thoroughly? Whether this is done or not almost completely depends on how much of a hassle it would be to add to the release process.

---

_Comment by @ghost on 2019-03-22 21:58_

I just checked this against alpine since it is used in .travis.yaml. The package llvm-dev would be required. The main problem is that `-Z` flags requires nightly rust, which is not available as an alpine package.


---

_Comment by @BurntSushi on 2019-04-14 17:40_

Thanks for suggesting this. I just briefly tried this out, and while I could get it to work, I couldn't really see any noticeable performance improvement. Moreover, this would complicate and prolong the release process by quite a bit. Not only does ripgrep need to be built twice, but it needs to get some sizable corpus on which to search a number of times. Overall, I don't think it's worth it.

---

_Closed by @BurntSushi on 2019-04-14 17:40_

---

_Label `question` added by @BurntSushi on 2019-04-14 17:40_

---

_Comment by @ArniDagur on 2019-08-15 20:56_

As of [Rust 1.37](https://blog.rust-lang.org/2019/08/15/Rust-1.37.0.html#profile-guided-optimization), PGO is stable.

---

_Comment by @zamazan4ik on 2023-11-27 03:47_

I didn't finish all the tests but I already have some preliminary results.

On my Linux machine (Fedora 39, AMD Ryzen 5900x with disabled Turbo boost, 48 Gib RAM, Rust 1.74, with `ripgrep` from the latest `master` branch with commit `cd5440fb6230f72ab598916c1c5ab96686541d47`) I get the following results:

```
hyperfine --warmup 5 --min-runs 20 '../target/rg_release -n "PM_RESUME" data/linux' '../target/rg_release_with_lto -n "PM_RESUME" data/linux' '../target/rg_optimized_with_lto -n "PM_RESUME" data/linux'
Benchmark 1: ../target/rg_release -n "PM_RESUME" data/linux
  Time (mean ± σ):     137.2 ms ±   3.4 ms    [User: 529.9 ms, System: 986.1 ms]
  Range (min … max):   129.3 ms … 142.4 ms    21 runs

Benchmark 2: ../target/rg_release_with_lto -n "PM_RESUME" data/linux
  Time (mean ± σ):     131.8 ms ±   2.2 ms    [User: 492.7 ms, System: 968.0 ms]
  Range (min … max):   126.9 ms … 135.8 ms    22 runs

Benchmark 3: ../target/rg_optimized_with_lto -n "PM_RESUME" data/linux
  Time (mean ± σ):     125.1 ms ±   2.9 ms    [User: 406.6 ms, System: 975.1 ms]
  Range (min … max):   119.5 ms … 132.5 ms    23 runs

Summary
  ../target/rg_optimized_with_lto -n "PM_RESUME" data/linux ran
    1.05 ± 0.03 times faster than ../target/rg_release_with_lto -n "PM_RESUME" data/linux
    1.10 ± 0.04 times faster than ../target/rg_release -n "PM_RESUME" data/linux
```
where:

* `rg_release` - default Release build with `cargo build --release`
* `rg_release_with_lto` - default Release build with `cargo build --release` but with enabled `codegen-units = 1` and `lto = true`
* `rg_optimized_with_lto` - default Release build with `cargo build --release` but with enabled `codegen-units = 1` and `lto = true` and optimized with PGO

As a PGO training set, I used Ripgrep's benchsuite.

Right now I cannot provide you with all the results since on my machine for some unknown yet reasons the provided `benchsuite` script doesn't work (it complains about missing dependencies even if they are downloaded), so I ran all the commands manually.

I ran several other commands from the bench suite - the performance improvements were near the same in the tested-by-me cases.

I specially built ripgrep without `pcre` since it's a system dependency and cannot be easily PGOed. Since I bench PGO for Ripgrep, I decided to reduce external DLLs influence as much as I could. For someone, it still could be interesting since some people can build Ripgrep without PCRE support.

I have some thoughts about these results:

* I've seen a [discussion](https://github.com/BurntSushi/ripgrep/issues/413) before that LTO doesn't bring huge improvements. According to my tests, 5% improvement is a huge improvement (even if we are not talking about the binary size). So I think the LTO decision should be estimated (at least enable it in an additional `profile.release-lto` Cargo profile)
* PGO shows measurable improvements too. More tests should be evaluated with PGO - maybe some cases are pessimized with PGO (who knows). Especially would be especially interesting to recompile with PGO the PCRE dependency too.

5-10% improvement for some people is really important. My main use-case for that - (rip)grepp'ing hundreds of logs on our log storage nodes with some non-trivial patterns. Some queries could run for tens of minutes/hours, and even a few percent of performance is definitely worth it (even if we need to recompile ripgrep - it's not a problem).

@BurntSushi Should I create a separate issue/discussion for the topic?

---

_Comment by @BurntSushi on 2023-11-28 02:34_

@zamazan4ik See https://github.com/BurntSushi/ripgrep/commit/b6bac8484e5466d332640c4022951061bb865329

Basically, I'm still not convinced. I only tested LTO and not PGO, but PGO does not sound like something I'm keen on maintaining.

In any case, I've at least added a `release-lto` profile to ripgrep, but stopped short of using it in the actual release binaries.

---
