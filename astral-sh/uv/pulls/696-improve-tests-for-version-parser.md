```yaml
number: 696
title: improve tests for version parser
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/version/test
created_at: 2023-12-18T22:36:53Z
updated_at: 2023-12-19T17:25:33Z
url: https://github.com/astral-sh/uv/pull/696
synced_at: 2026-01-12T16:04:08Z
```

# improve tests for version parser

---

_@BurntSushi_

The high level goal here is to improve the tests for the version parser. Namely, we now check not just that version strings parse successfully, but that they parse to the expected result.

We also do a few other cleanups. Most notably, `Version` is now an opaque type so that we can more easily change its representation going forward.

Reviewing commit-by-commit is suggested. :-)

---

_Label `internal` added by @BurntSushi on 2023-12-18 22:36_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-18 22:36_

---

_Review requested from @konstin by @BurntSushi on 2023-12-18 22:36_

---

_@charliermarsh reviewed on 2023-12-18 23:13_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/lib.rs`:52 on 2023-12-18 23:13_

👍 We used to do this via using `rustfmt` nightly... Now I occasionally run IntelliJ's "Organize Imports" command which leads to some of the organization we have.

---

_@charliermarsh reviewed on 2023-12-18 23:13_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/specifier.rs`:75 on 2023-12-18 23:13_

Did this disappear by accident?

---

_@charliermarsh approved on 2023-12-18 23:16_

---

_@charliermarsh reviewed on 2023-12-18 23:16_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/specifier.rs`:75 on 2023-12-18 23:16_

Only "blocking" feedback -- double-checking that this was intentional.

---

_@BurntSushi reviewed on 2023-12-18 23:34_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/specifier.rs`:75 on 2023-12-18 23:34_

No actually! I had re-written it to use the public accessors on `Version`, and that triggered an unused code warning. I guess the way it is written today (since `low` is assigned to in place) doesn't trigger it. In any case, `low` isn't used after this point. It _looks_ like it should, but if you do, the tests fail due to a version resolution being impossible.

Since this code was a no-op today, and "fixing" it in the obvious way resulted in test failures and because I didn't quite understand what the right path was, I just decided to remove it. I meant to flag it in the commit message, but clearly forgot.

---

_@BurntSushi reviewed on 2023-12-18 23:37_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/lib.rs`:52 on 2023-12-18 23:37_

Yeah I'm still doing it by hand like a nincompoop. I should just figure out a flow to make `rustfmt` do it in an opt-in fashion.

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:356 on 2023-12-19 08:14_

Doesn't the compiler inline trivial methods by itself?

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:644 on 2023-12-19 08:16_

TIL :bulb: 

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:1271 on 2023-12-19 08:20_

Can this be `assert_eq!`? I'm surprised clippy doesn't complain.

---

_@konstin approved on 2023-12-19 08:20_

---

_@MichaReiser reviewed on 2023-12-19 08:37_

---

_Review comment by @MichaReiser on `crates/pep440-rs/src/version.rs`:356 on 2023-12-19 08:37_

Inside a crate, I think yes. Across crates: Depends on the enabled LTO optimisations. But I agree, I don't think it's necessary to mark all these functions as `#[inline]` except we have evidence that inlining some of them in hot-paths is beneficial

---

_@BurntSushi reviewed on 2023-12-19 16:07_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:356 on 2023-12-19 16:07_

Right. Across crates, unless a function would otherwise be monomorphized because of generics (not the case here), a function needs an `#[inline]` or `#[inline(always)]` annotation in order to get inlined. As @MichaReiser mentions though, the exception is LTO. When LTO is enabled, cross crate inlining becomes available.

The `#[inline]` annotations here are warranted IMO for the following reasons:

* It reduces our reliance on LTO for inlining trivial functions. We control our own destiny. If we were always using fat LTO, you could make an argument that we don't need this. But we use thin LTO (currently, and I think we should probably continue to do so).
* These are trivial non-generic functions and this is an `#[inline]` annotation and not an `#[inline(always)]` annotation. The former is a hint, true, but not just a hint. Here it's more used as "I'm telling the compiler to make these _available_ for inlining."

I'm personally somewhat relaxed about `#[inline]`. It's the `#[inline(always)]` annotations that I try to reserve for "we have a good reason to do this."

This is good reading: https://matklad.github.io/2021/07/09/inline-in-rust.html

---

_@BurntSushi reviewed on 2023-12-19 16:19_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:644 on 2023-12-19 16:19_

So when I did this I thought I was very clever. But it turns out that Insta uses `{:#?}` in its output, so this makes all the snapshots uses this bloated alternate formatting.

I think you were the one that made this terser right? Did you do that for the snapshots or for something else? Because I do really want to keep the terser formatting. I only really wanted the more verbose output for tests inside this crate because it's useful to see when test failures occur.

If you're not fond of the snapshots getting bloated again, I can find another way to go about this.

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1271 on 2023-12-19 16:20_

It can be, but the error message isn't as good because it uses `{:?}` and not `{:#?}`.

---

_@BurntSushi reviewed on 2023-12-19 16:20_

---

_@BurntSushi reviewed on 2023-12-19 17:03_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:644 on 2023-12-19 17:03_

Okay, I've worked around this with a new test-only type that gives me the verbose debug representation and left the `Debug` impl that was there alone.

---

_Merged by @BurntSushi on 2023-12-19 17:25_

---

_Closed by @BurntSushi on 2023-12-19 17:25_

---

_Branch deleted on 2023-12-19 17:25_

---
