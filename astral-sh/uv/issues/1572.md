```yaml
number: 1572
title: What about pixi and rip?
type: issue
state: closed
author: olivier-lacroix
labels:
  - question
assignees: []
created_at: 2024-02-17T04:29:50Z
updated_at: 2025-03-15T11:55:40Z
url: https://github.com/astral-sh/uv/issues/1572
synced_at: 2026-01-10T03:50:29Z
```

# What about pixi and rip?

---

_Issue opened by @olivier-lacroix on 2024-02-17 04:29_

Hello there,

I have been looking forward to improved package management for python, and am excited by the uv announcement. I have also been following with interest the developments made by the prefix team in [pixi](https://github.com/prefix-dev/pixi) and [rip](https://github.com/prefix-dev/rip).

Benchmarking uv with rip seems to show, anecdotally, that rip is as fast or faster than uv (I may be doing this completely wrong though):

```bash
❯ hyperfine --warmup 3 'cargo r install dagster /tmp/ripenv' 'source /tmp/uvenv/bin/activate;uv pip install --reinstall dagster --no-cache'
Benchmark 1: cargo r install dagster /tmp/ripenv
  Time (mean ± σ):      6.959 s ±  0.236 s    [User: 9.346 s, System: 0.370 s]
  Range (min … max):    6.643 s …  7.335 s    10 runs

Benchmark 2: source /tmp/uvenv/bin/activate;uv pip install --reinstall dagster --no-cache
  Time (mean ± σ):      7.829 s ±  0.819 s    [User: 0.808 s, System: 1.080 s]
  Range (min … max):    6.183 s …  8.809 s    10 runs

Summary
  cargo r install dagster /tmp/ripenv ran
    1.13 ± 0.12 times faster than source /tmp/uvenv/bin/activate;uv pip install --reinstall dagster --no-cache
```

Pixi and uv seem to have very similar goals (albeit pixi ambitions to cater to other languages than python). 

I am wondering about the opportunities to share, at least, (some) underlying libraries to achieve these goals faster.


---

_Label `question` added by @zanieb on 2024-02-17 04:31_

---

_Comment by @zanieb on 2024-02-17 05:01_

Hi!

We've talked to each other before and there's definitely more that we can do together, but it's worth pointing out that there has already been collaboration — it's just not obvious.

For example, they are [using a couple](https://github.com/prefix-dev/rip/blob/main/crates/rattler_installs_packages/Cargo.toml#L42-L43) foundational crates that @konstin published e.g.

- [pep440_rs](https://docs.rs/pep440_rs/latest/pep440_rs/)
- [pep508_rs](https://docs.rs/pep508_rs/latest/pep508_rs/)

And we are using an HTTP crate from them:

- [async_http_range_reader](https://github.com/prefix-dev/async_http_range_reader)

Additionally, all of our [test scenarios](https://github.com/astral-sh/uv/blob/main/crates/uv/tests/pip_install_scenarios.rs) have also been open source at [packse](https://github.com/zanieb/packse) long before we announced `uv` — they recently made use of a couple of these in their [test suite](https://github.com/prefix-dev/rip/pull/141). I'd love to see these test cases adopted across the Python packaging ecosystem, e.g. they would be useful to ensure behavior confirms in `pip`, `poetry`, etc.

One barrier to collaboration is that they use a different resolver than us; they wrote [resolvo](https://github.com/mamba-org/resolvo) but we are using [PubGrub](https://github.com/pubgrub-rs/pubgrub). These solvers have some fundamental differences, and during our design phase we decided to commit to PubGrub instead of a `libsolv`-style solver. However, we're working closely with the `pubgrub-rs` maintainers who have also expressed interest in working with their team:

- https://github.com/mamba-org/resolvo/issues/2
- https://github.com/mamba-org/resolvo/issues/9#issuecomment-1857703964

And work on `pubgrub-rs` has far reaching implications for resolvers across language ecosystems, e.g. the authors would love to see it used in Cargo some day:

- https://github.com/rust-lang/cargo/issues/5284

We have more [crates](https://github.com/astral-sh/uv/tree/main/crates) in uv that we can publish for shared-use once we stabilize them, but everything is permissively licensed and open source.

We're excited to have everything public now — it's going to make collaboration a lot easier.

Regarding benchmarks, it's tough to create a robust benchmark. If you're interested in exploring it, I'd recommend adding `rip` support to our [bench script](https://github.com/astral-sh/uv/tree/main/scripts/bench).



---

_Comment by @charliermarsh on 2024-02-17 05:22_

Thanks, I appreciate this! Agree with everything @zanieb wrote above. We've chatted with various folks on the Prefix team at different points in time, and my feeling is that we just have different goals and focus areas (though with some overlap of course). But at the very least, there will still be opportunities for us to collaborate on some of the underlying libraries (like those mentioned above).

On the benchmarks... Benchmarking is tricky. We put a lot of time into the benchmark script in the repo. As a heads up, the command above seems to be comparing `rip` _with_ caching against `uv` _without_ caching. (`rip` has a persistent cache, but doesn't currently expose command-line flags to modify it, based on my read.) I think it's also using a debug build for `rip` but a release build for `uv`. The details are hard to get right :joy:

I'm happy to produce some rigorous benchmarks if folks are interested -- we could probably add `rip` to our benchmark script. I just re-ran the command you used above, comparing release builds and clearing the cache between invocations:

```
❯ hyperfine --prepare "./target/release/uv venv" "./target/release/uv pip install -r requirements.in -n" --prepare "rm -rf ~/Library/Caches/rattler/pypi/ .venv" "../rip/target/release/rip install dagster .venv" --warmup 10 --runs 20
Benchmark 1: ./target/release/uv pip install -r requirements.in -n
  Time (mean ± σ):      1.267 s ±  0.129 s    [User: 0.419 s, System: 0.807 s]
  Range (min … max):    1.070 s …  1.573 s    20 runs

Benchmark 2: ../rip/target/release/rip install dagster .venv
  Time (mean ± σ):      3.379 s ±  0.244 s    [User: 0.851 s, System: 1.003 s]
  Range (min … max):    2.987 s …  3.927 s    20 runs

Summary
  './target/release/uv pip install -r requirements.in -n' ran
    2.67 ± 0.33 times faster than '../rip/target/release/rip install dagster .venv'
```

So that shows something like a > 2.5x speed-up with `uv`, but it'll vary across platform, machine, etc. (I took the median of three runs).


---

_Comment by @olivier-lacroix on 2024-02-17 10:20_

Thanks @charliermarsh and @zanieb !

Started to add pixi in the bench script in https://github.com/astral-sh/uv/pull/1581

---

_Comment by @wolfv on 2024-02-17 11:55_

Hey 👋 

Indeed, the differences are nicely pointed out by Charlie and Zanie and we're just as happy to collaborate on some of the underlying issues. 

I also did some benchmarking and indeed `uv` is a little faster, for the following reasons:

- `rip` rigorously checks SHA256 hashes of the RECORD files inside packages, and `uv` seems to skip that. `pip` also skips that (which is why some Python packages actually ship with broken hashes)
- `rip`'s cache is currently storing wheels as zip files while `uv` stores the extracted files. This is something we need to change in `rip`, but shouldn't be particularly hard (we pay the cost of re-extracting the packages each install time). We also already do that for `conda` packages.
- We check if new packages are on PyPI everytime instead of adhering to the 10 minutes Max-Age. This is behavior that we adopted from `pip` – we'll make this configurable in `rip`.

And lastly `rip` and `uv` share some performance problems, e.g. `urllib botocore` is pretty tough to resolve.

Anyways, as Charlie and Zanie mentioned we do share a bunch of underlying crates and hopefully we can share more going forward :)

---

_Comment by @notatallshaw on 2024-02-17 16:54_

On the topic of resolver benchmarks, can I suggest that people take a look at the approach [pip-resolver-benchmarks](https://github.com/pradyunsg/pip-resolver-benchmarks) takes.

My understanding is the library is experimental right now, but being able to collect full real world scenarios at a point in time and elliminate network variability (without just relying on cache) are both very helpful.

---

_Comment by @zanieb on 2024-02-17 16:56_

Thanks for the reference! That project inspired our [packaging scenarios](https://github.com/zanieb/packse) — we're very interested in being able to use those benchmark scenarios as inputs.

And thanks for chiming in Wolf!

Here are some issues regarding the behaviors mentioned:

- https://github.com/astral-sh/uv/issues/888
- https://github.com/astral-sh/uv/issues/505

---

_Comment by @tekumara on 2024-02-18 07:21_

> we decided to commit to PubGrub instead of a libsolv-style solver

just a question to satisfy my own intellectual curiosity around resolvers, what made you settle on PubGrub over libsolv or alternatives?

---

_Comment by @JelleZijlstra on 2024-02-19 19:38_

> just a question to satisfy my own intellectual curiosity around resolvers, what made you settle on PubGrub over libsolv or alternatives?

FYI Charlie talks about that here:

https://discuss.python.org/t/uv-another-rust-tool-written-to-replace-pip/46039/56

---

_Comment by @ruben-arts on 2024-02-21 17:02_

Following up on @wolfv's response, 

We've decided to integrate `uv` into `pixi` to avoid double work and to be able to contribute to the same code base. This allows us to refocus on `conda` specific work. Killing `rip` as of immediately, but taking what we've learned and hopefully being able to support `uv` with it.  

---

_Comment by @zanieb on 2024-02-21 17:24_

Thanks for the update! I'm going to close this as I think all the major questions here have been answered.

---

_Closed by @zanieb on 2024-02-21 17:24_

---

_Comment by @EED85 on 2025-03-03 08:59_

Hi @ruben-arts,

> We've decided to integrate uv into pixi

Did you have success?
Our packages are built on a conda standard, and we're looking to expedite the creation of conda environments.
Pixi with UV integrations seems very promising!

This article sounds great: https://prefix.dev/blog/uv_in_pixi


---

_Comment by @straygar on 2025-03-04 15:54_

@EED85 This is done, all `[pypi-dependencies]` are installed using `uv`.

---

_Comment by @cesaryuan on 2025-03-15 08:13_

> [@EED85](https://github.com/EED85) This is done, all `[pypy-dependencies]` are installed using `uv`.

Is there a misspelling of `[pypi-dependencies]` here?

---

_Comment by @straygar on 2025-03-15 11:55_

@cesaryuan yup, just fixed the typo.

---
