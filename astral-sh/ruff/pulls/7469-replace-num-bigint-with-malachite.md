```yaml
number: 7469
title: Replace num_bigint with malachite
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
base: main
head: malachite
created_at: 2023-09-17T18:54:57Z
updated_at: 2023-10-23T09:28:07Z
url: https://github.com/astral-sh/ruff/pull/7469
synced_at: 2026-01-12T15:55:24Z
```

# Replace num_bigint with malachite

---

_@konstin_

[malachite](https://www.malachite.rs/) is an arbitrary precision crate that unlike num_bigint implement as small integer optimization. This avoids allocating a `Vec` for every single number parse and instead only allocates for rare unusually large numbers.

This is also a correctness improvement since we'd previously just ignore the higher parts of unreasonably large numbers.

The disadvantage is that malachite is slow to compile.

TODO: Is the LGPL that malachite uses ok for ruff?

---

_Label `internal` added by @konstin on 2023-09-17 18:54_

---

_Comment by @charliermarsh on 2023-09-17 19:22_

My understanding is that we _can_ use LGPL dependencies, but we need to provide the copyright notice (as always) and make the source code of our LGPL dependencies available (it sounds like providing a link to the source is sufficient to satisfy this). If we modify the underlying library, those modifications would need to be licensed under LGPL.

(I'm not a lawyer, this is based off my own research just now.)


---

_Comment by @github-actions[bot] on 2023-09-17 19:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @codspeed-hq[bot] on 2023-09-18 09:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/malachite)

### Merging #7469 will **improve performances by 5.76%**

<sub>Comparing <code>malachite</code> (6bd83d3) with <code>main</code> (94b68f2)</sub>



### Summary

`⚡ 2` improvements
`✅ 23` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `malachite` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[unicode/pypinyin.py]` | 620 µs | 604.1 µs | +2.62% |
| ⚡ | `lexer[large/dataset.py]` | 9.8 ms | 9.3 ms | +5.76% |


---

_Comment by @MichaReiser on 2023-09-18 15:15_

The formatter result looks wrong. The formatting performance can only be regressed by this PR if the lexing performance regresses too. 

It seems that malachite's parse method allocates internally? 

![image](https://github.com/astral-sh/ruff/assets/1203881/078ed46e-7a07-4cb4-a768-ba5f07d07cae)


---

_Comment by @konstin on 2023-09-18 15:23_

> It seems that malachite's parse method allocates internally?

i've added the fast path for that reason, i'm rather certain it should not allocate or at least not sufficiently often to show up in the flamegraph. Could the flamegraph be outdated?

---

_Comment by @MichaReiser on 2023-09-18 15:27_

> > It seems that malachite's parse method allocates internally?
> 
> i've added the fast path for that reason, i'm rather certain it should not allocate or at least not sufficiently often to show up in the flamegraph. Could the flamegraph be outdated?

You can run a lexer profile locally

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:269 on 2023-09-18 15:29_

This method is never called with `Radix::Decimal`. You need to add your fast path to `lex_decimal_number`

---

_@MichaReiser reviewed on 2023-09-18 15:29_

---

_@konstin reviewed on 2023-09-18 15:34_

---

_Review comment by @konstin on `crates/ruff_python_parser/src/lexer.rs`:269 on 2023-09-18 15:34_

:facepalm: thanks that makes much more sense

---

_Comment by @konstin on 2023-09-18 15:35_

> You can run a lexer profile locally

i know, but the build takes so long and then i have to make sure i have a quiet environment for running the benchmarks, but in this case i will

---

_@MichaReiser reviewed on 2023-09-18 16:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:348 on 2023-09-18 16:09_

Nit: We should know if the number is negative, meaning we can have different paths for integers and unsigned integers. 

---

_@MichaReiser reviewed on 2023-09-18 16:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:359 on 2023-09-18 16:10_

Nit: This check is a bit awkward, but you have to make sure your code still passes it

---

_@konstin reviewed on 2023-09-18 17:03_

---

_Review comment by @konstin on `crates/ruff_python_parser/src/lexer.rs`:348 on 2023-09-18 17:03_

i don't think it's worth the complexity

---

_Comment by @konstin on 2023-09-18 17:04_

The lexer now improves consistently, not sure about that formatter regression

![image](https://github.com/astral-sh/ruff/assets/6826232/96948829-fd2d-4d99-92c9-488a07cd6c6f)


---

_Marked ready for review by @konstin on 2023-09-18 17:04_

---

_Comment by @charliermarsh on 2023-09-18 17:07_

`formatter[large/dataset.py]` has been really flakey lately. I'm not sure why. It's oscillating ~5% on a bunch of PRs.

---

_Comment by @MichaReiser on 2023-09-18 17:16_

Yeah. I think something with baselines is wrong... 


---

_Closed by @MichaReiser on 2023-09-18 18:48_

---

_Reopened by @MichaReiser on 2023-09-18 18:48_

---

_Review comment by @MichaReiser on `Cargo.lock`:1419 on 2023-09-18 18:52_

It's unfortunate that it pulls in all these dependencies without an option to disable them

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:123 on 2023-09-18 19:00_

Nit: What's the reason for returning an `Integer` here? Converting to a `u16` seems reasonable (because anything larger isn't a valid file permission anyway)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:92 on 2023-09-18 19:04_

A `i64` was probably sufficient to represent any reasonable value (any value outside of that range certainly results in an out of memory exception, assuming Python can even handle it)

---

_@MichaReiser approved on 2023-09-18 19:06_

Have you considered other arbitrary precision crates like e.g. rug?

https://github.com/tczajka/bigint-benchmark-rs (all of these seem to have very few downloads, except num bigint)

---

_Review comment by @konstin on `Cargo.lock`:1419 on 2023-09-19 09:37_

https://github.com/mhogrefe/malachite/issues/26

---

_@konstin reviewed on 2023-09-19 09:37_

---

_Comment by @konstin on 2023-09-19 09:44_

> Have you considered other arbitrary precision crates like e.g. rug?

i quickly looked but i found none that looked better than malachite. it seems there's num_bigint and than there's a lot of crates with low usage numbers. i find malachite to be much more ergonomic than num_bigint

---

_@konstin reviewed on 2023-09-19 09:45_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:123 on 2023-09-19 09:45_

the diagnostic would be incorrect if we truncated the number

---

_@konstin reviewed on 2023-09-19 09:50_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:92 on 2023-09-19 09:50_

python is perfectly happy to do `list(range(10))[:2**10000]`. it's not reasonable from a user perspective but there's also no reason not to support it in ruff

---

_Review requested from @charliermarsh by @konstin on 2023-09-19 10:03_

---

_Comment by @konstin on 2023-09-19 10:03_

CC @charliermarsh are you fine with merging given that this is LGPL?

---

_Comment by @charliermarsh on 2023-09-19 12:47_

@konstin - Looking into it...

---

_Comment by @konstin on 2023-09-21 14:57_

We've decided that it's not worth adding an LPGL dependency just for bigint support which don't really need, we don't have any cases where we need a large number parsed. Instead, we plan on something like the following:

```rust
enum BigInt {
    Small(i64),
    Large(Box<str>)
}
````

We parse only small numbers and keep the large numbers as a (normalized) string. Comparisons like `num == 0` only need to check against the small variant, but we still keep the large variant around for correct diagnostics and unparsing.

---

_Closed by @konstin on 2023-09-21 14:57_

---

_Comment by @charliermarsh on 2023-09-21 15:08_

For posterity: our assessment is that we _can_ add an LGPL dependency, with the requirements being that we (1) include the license for the dependency, (2) provide access to the LGPL library's source code, and (3) provide a means by which users can replace the LGPL dependency in the compiled binary. My understanding is that (3) can be satisfied by providing access to the Ruff source code itself, since users can then download the source code, swap out the library and re-compile. In practice, I believe that the crates.io link would also satisfy (2).

So, anyway, I do believe we can add LGPL dependencies while remaining MIT licensed ourselves. But it doesn't feel worth it to me to introduce these requirements and any uncertainties for a minor improvement.

---

_Comment by @cmpute on 2023-10-23 09:28_

I might come late but I would like to advertise the [`dashu`](https://github.com/cmpute/dashu) crate created by me. It's Apache-MIT licensed, and it has small integer optimizations.

---
