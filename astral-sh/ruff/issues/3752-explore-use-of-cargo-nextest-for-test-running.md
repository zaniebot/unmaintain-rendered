```yaml
number: 3752
title: "Explore use of `cargo nextest` for test running"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-03-26T21:10:01Z
updated_at: 2023-06-29T00:15:27Z
url: https://github.com/astral-sh/ruff/issues/3752
synced_at: 2026-01-10T11:09:46Z
```

# Explore use of `cargo nextest` for test running

---

_Issue opened by @charliermarsh on 2023-03-26 21:10_

We should profile this to understand whether it's faster for our project.

See: https://twitter.com/boshen_c/status/1639878432820727809.

See: https://github.com/Boshen/oxc/blob/d4af69930ceb9f083e6358f60b0967abb3bfc5f5/.github/workflows/check.yaml#L104-L115.


---

_Label `internal` added by @charliermarsh on 2023-03-26 21:10_

---

_Comment by @JonathanPlasse on 2023-03-26 22:15_

Nextest does not run doctest so I disabled doctest by replacing `/// ```rust` with `/// ```no_run`.

warm perfs
```console
❯ hyperfine 'cargo nextest run --all --all-features'
Benchmark 1: cargo nextest run --all --all-features
  Time (mean ± σ):      6.459 s ±  0.115 s    [User: 63.093 s, System: 6.695 s]
  Range (min … max):    6.292 s …  6.700 s    10 runs

❯ hyperfine 'cargo test --all --all-features'
Benchmark 1: cargo test --all --all-features
  Time (mean ± σ):     11.376 s ±  0.056 s    [User: 43.252 s, System: 9.700 s]
  Range (min … max):   11.296 s … 11.509 s    10 runs
```



---

_Comment by @JonathanPlasse on 2023-03-26 22:27_

I cannot get to run `cargo test --doc`.
I have this error:
```console
❯ cargo test --doc
    Finished test [unoptimized + debuginfo] target(s) in 0.19s
   Doc-tests ruff

running 11 tests
…
test src/map_codes.rs - map_codes::Entry::parse (line 283) ... FAILED

failures:

---- src/map_codes.rs - map_codes::Entry::parse (line 283) stdout ----
error: expected one of `.`, `;`, `?`, `}`, or an operator, found `=>`
 --> src/map_codes.rs:284:23
  |
3 | (Pycodestyle, "E101") => Rule::MixedSpacesAndTabs,
  |                       ^^ expected one of `.`, `;`, `?`, `}`, or an operator

error: aborting due to previous error

Couldn't compile the test.

failures:
    src/map_codes.rs - map_codes::Entry::parse (line 283)

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.03s

error: doctest failed, to rerun pass `-p ruff_macros --doc`
```

---

_Comment by @konstin on 2023-03-28 11:31_

Strange, fixed in https://github.com/charliermarsh/ruff/pull/3766

---

_Comment by @MichaReiser on 2023-03-28 12:35_

Thank you @JonathanPlasse for measuring the wall-times. Did you create the measurements locally or on a GitHub Action? 

My reaction is that it's probably not worth using `nextest` on CI **today** because
* The majority of the time is spent compiling
* The huge benefit of `nextest` is that it does a better job at concurrently running jobs. This is less important when using the default Github Action runners, because they [have only two cores](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources), 
* Installing `nextest` will add  a few extra seconds 
* It increases the complexity of the CI job (installing, calling nextest, calling `cargo test` for doctests)

But we may want to reconsider in the future, when we have more tests.

---

_Closed by @charliermarsh on 2023-06-29 00:15_

---

_Comment by @charliermarsh on 2023-06-29 00:15_

We've explored it a bit -- not using for now, so closing.

---
