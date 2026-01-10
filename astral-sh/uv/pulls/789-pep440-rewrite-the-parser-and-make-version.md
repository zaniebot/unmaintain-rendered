```yaml
number: 789
title: "pep440: rewrite the parser and make version comparisons cheaper"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/version-opt
created_at: 2024-01-04T19:55:37Z
updated_at: 2024-01-09T16:37:44Z
url: https://github.com/astral-sh/uv/pull/789
synced_at: 2026-01-10T15:44:44Z
```

# pep440: rewrite the parser and make version comparisons cheaper

---

_Pull request opened by @BurntSushi on 2024-01-04 19:55_

This PR builds on #780 by making both version parsing faster, and
perhaps more importantly, making version comparisons much faster.
Overall, these changes result in a considerable improvement for the
`boto3.in` workload. Here's the status quo:

```
$ time puffin pip-compile --no-build --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/requirements/boto3.in
Resolved 31 packages in 34.56s

real    34.579
user    34.004
sys     0.413
maxmem  2867 MB
faults  0
```

And now with this PR:

```
$ time puffin pip-compile --no-build --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/requirements/boto3.in
Resolved 31 packages in 9.20s

real    9.218
user    8.919
sys     0.165
maxmem  463 MB
faults  0
```

This particular workload gets stuck in pubgrub doing resolution, and
thus benefits mightily from a faster `Version::cmp` routine. With that
said, this change does also help a fair bit with "normal" runs:

```
$ hyperfine -w10 \
    "puffin-base pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in" \
    "puffin-cmparc pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in"
Benchmark 1: puffin-base pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in
  Time (mean ± σ):     337.5 ms ±   3.9 ms    [User: 310.5 ms, System: 73.2 ms]
  Range (min … max):   333.6 ms … 343.4 ms    10 runs

Benchmark 2: puffin-cmparc pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in
  Time (mean ± σ):     189.8 ms ±   3.0 ms    [User: 168.1 ms, System: 78.4 ms]
  Range (min … max):   185.0 ms … 196.2 ms    15 runs

Summary
  puffin-cmparc pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in ran
    1.78 ± 0.03 times faster than puffin-base pip-compile --cache-dir ~/astral/tmp/cache/ -o /dev/null ./scripts/benchmarks/requirements.in
```

There is perhaps some future work here (detailed in the commit
messages), but I suspect it would be more fruitful to explore ways of
making resolution itself and/or deserialization faster.

Fixes #373, Closes #396


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-04 19:56_

---

_Review requested from @konstin by @BurntSushi on 2024-01-04 19:56_

---

_@charliermarsh reviewed on 2024-01-04 19:57_

Can't wait to read this, I feel like I'm gonna learn a lot.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-04 19:58_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:866 on 2024-01-05 01:35_

Nit: I think `KINDS` got renamed to `SPELLINGS`.

---

_@charliermarsh reviewed on 2024-01-05 01:35_

---

_@charliermarsh reviewed on 2024-01-05 01:37_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:1754 on 2024-01-05 01:37_

That's neat.

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:1461 on 2024-01-05 01:44_

So in this case, what ultimately happens to the separator? Now that we ate-and-then-reset for pre, then post, then dev? Like if you had a separator at the end of the version.

---

_@charliermarsh reviewed on 2024-01-05 01:44_

---

_@charliermarsh reviewed on 2024-01-05 01:44_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:1701 on 2024-01-05 01:44_

Interesting, so this is optimized for the case in which most versions don't contain any of the alphabetical characters?

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:749 on 2024-01-05 01:49_

This is ridiculously good! And the commentary is excellent!

---

_@charliermarsh reviewed on 2024-01-05 01:49_

---

_@charliermarsh reviewed on 2024-01-05 01:53_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:509 on 2024-01-05 01:53_

Very interesting pattern.

---

_@charliermarsh reviewed on 2024-01-05 01:54_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:771 on 2024-01-05 01:54_

Interesting, this seems quite doable (but perhaps not critical).

---

_@charliermarsh reviewed on 2024-01-05 01:55_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:245 on 2024-01-05 01:55_

I will ask a dumb question: why `Arc` and not `Box`?

---

_@charliermarsh reviewed on 2024-01-05 01:56_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:245 on 2024-01-05 01:56_

Oh, is it because `Arc` allows for cheap cloning...?

---

_@charliermarsh approved on 2024-01-05 01:56_

This is really quite an incredible PR, and you did a great job of breaking down some very sophisticated optimizations and decision-making. I loved reading it, and the results are remarkable!

---

_Review comment by @zanieb on `crates/pep440-rs/src/version.rs`:1253 on 2024-01-05 02:44_

I'm surprised you don't explicitly note what happens if it doesn't match that format since your docstrings are so thorough. Is it just obviously implied that it returns `None`?

---

_@zanieb reviewed on 2024-01-05 02:44_

---

_@zanieb reviewed on 2024-01-05 02:46_

---

_Review comment by @zanieb on `crates/pep440-rs/src/version.rs`:509 on 2024-01-05 02:46_

🤯 cool

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:245 on 2024-01-05 13:38_

Do we want the `Arc` here or in `PubGrubVersion`? That is, would it have any disadvantages to only use it in `PubGrubVersion` so `Version` itself remains the "real" type / does the copy on write make a noticeable impact?

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:771 on 2024-01-05 13:49_

Looking at the callers of `.release()`, we don't seem to have any arbitrary indexing, mostly either `.iter()` or `.to_vec()`, so would a `impl Iterator<u64>` also do?

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:1221 on 2024-01-05 14:16_

Should we `_` -> `PatternErrorKind::WildcardNotTrailing` here?

---

_Review comment by @konstin on `crates/pep440-rs/src/version.rs`:1642 on 2024-01-05 14:37_

How does this compare to a `SmallVec::<[u64; 4]>`?

---

_@konstin approved on 2024-01-05 14:46_

woah

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1461 on 2024-01-05 16:05_

This runs at the end of the top-level parse routine:

```
        if !self.is_done() {
            let remaining = String::from_utf8_lossy(&self.v[self.i..]).into_owned();
            let version = self.into_pattern().version;
            return Err(ErrorKind::UnexpectedEnd { version, remaining }.into());
        }
```

So basically, if by the time you have nothing left to try you still have stuff left to parse, you know you've hit a dead end and an invalid version.

---

_@BurntSushi reviewed on 2024-01-05 16:05_

---

_@BurntSushi reviewed on 2024-01-05 16:07_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1701 on 2024-01-05 16:07_

Right. It saves us from needing to do a case insensitive comparison for every alphabetical version component.

I wouldn't be surprised if this didn't make much of a difference in practice, since most versions are handled by the "fast path" in the parser.

---

_@BurntSushi reviewed on 2024-01-05 16:20_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:771 on 2024-01-05 16:20_

You could very like make `impl Iterator<u64>` work, but it comes at the cost of convenience. See for example:

https://github.com/astral-sh/puffin/blob/9d51b22e189d972c7bfe6902ca250dea4e040664/crates/puffin-cli/src/python_version.rs#L31-L59

and

https://github.com/astral-sh/puffin/blob/9d51b22e189d972c7bfe6902ca250dea4e040664/crates/puffin-resolver/src/pubgrub/specifier.rs#L39-L41

There are maybe a few other spots where an `impl Iterator<u64>` would be inconvenient. But not bad.

I was also thinking about this from the perspective of this crate being an ecosystem crate. Using `&[u64]` is really nice. It's convenient. Composes well. Works with slice patterns.

---

_@BurntSushi reviewed on 2024-01-05 16:29_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:245 on 2024-01-05 16:29_

Yes, `Arc` makes cloning essentially free.

I think `Version` is fine with an `Arc` inside of it, and it's hard for me to conceive of a way that copy-on-write would hurt things. I would expect that the vast majority of the time, when `Arc::make_mut` is called, an extra copy is _not_ made. (Since it's done as part of building a new `Version`.) And even then, in the cases where `Arc::make_mut` does make a copy, the caller would have had to make a copy anyway if `Arc` weren't used.

I think there are really only two downsides to using `Arc` here. Firstly, it introduces an alloc in the happy path of building/parsing a version. Secondly, it requires using either `alloc` or `std`. I don't think the first makes too much of a difference, and I think the latter doesn't matter for this crate (unless you want to take in the direction of `core`-only, which would be quite tricky).

---

_@BurntSushi reviewed on 2024-01-05 16:31_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1253 on 2024-01-05 16:31_

That's fair. I added a sentence!

---

_@BurntSushi reviewed on 2024-01-05 16:34_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1221 on 2024-01-05 16:34_

I can do that. I think the idea is that all version-specific errors would be grouped into `PatternErrorKind::Version`, and thus, everything else would be an error associated with something wrong with it being a pattern. But since there's only one other variant (and that will probably never change), it's not a bad idea to explicitly spell it out.

---

_@BurntSushi reviewed on 2024-01-05 16:35_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1642 on 2024-01-05 16:35_

A lot less code. One fewer dependency. No use of `unsafe`. _Probably_ not much if any difference in perf. But I didn't test the last claim.

---

_@BurntSushi reviewed on 2024-01-05 16:37_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:1642 on 2024-01-05 16:37_

If `smallvec` were already a dependency of this crate, I probably would have used it. But this didn't feel like enough of a reason to add it all on its own IMO. Also, if this were a strictly internal crate with no plans to publish it back out into the world, I might have also used `smallvec`. But if this is going back out into the world, then I don't think it's worth passing on a dependency for `smallvec` here. IMO anyway.

---

_@BurntSushi reviewed on 2024-01-05 16:40_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:245 on 2024-01-05 16:40_

> does the copy on write make a noticeable impact?

It might help to clarify here: `Arc::make_mut` isn't quite just copy-on-write. It only makes a copy when the reference count is greater than 1. But if it's 1, and I suspect it usually will be in this context, then a mutable borrow with no copying can be handed out safely because exclusivity has been proven.

Typically copy-on-write implies something closer to "whenever one needs to write, you always copy first." And indeed, if that were the case here, it might make `Arc`-usage a little more suspicious.

---

_Merged by @BurntSushi on 2024-01-05 16:57_

---

_Closed by @BurntSushi on 2024-01-05 16:57_

---

_Branch deleted on 2024-01-05 16:57_

---

_@MichaReiser reviewed on 2024-01-09 16:31_

---

_Review comment by @MichaReiser on `crates/pep440-rs/src/version.rs`:620 on 2024-01-09 16:31_

Should we add a fast path here that avoids calling into `cmp_slow` if both `self.inner` and `other.inner` point to the same `Arc`? 

---

_@BurntSushi reviewed on 2024-01-09 16:37_

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version.rs`:620 on 2024-01-09 16:37_

Not sure. `cmp_slow` is designed to be called very infrequently (in practice), and I don't know the portion of comparisons that enter `cmp_slow` that have pointer equality internally. My prior is that the portion would be rather small, but I could be wrong. Especially for particular benchmarks that might hit `cmp_slow` more often than is intended. For example: https://github.com/astral-sh/puffin/pull/799

---
