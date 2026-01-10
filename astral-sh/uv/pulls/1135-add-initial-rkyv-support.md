```yaml
number: 1135
title: add initial rkyv support
type: pull_request
state: merged
author: BurntSushi
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: ag/rkyv-init
created_at: 2024-01-26T19:57:15Z
updated_at: 2024-01-28T18:12:21Z
url: https://github.com/astral-sh/uv/pull/1135
synced_at: 2026-01-10T15:39:03Z
```

# add initial rkyv support

---

_Pull request opened by @BurntSushi on 2024-01-26 19:57_

This PR adds initial support for [rkyv] to puffin. In particular,
the main aim here is to make puffin-client's `SimpleMetadata` type
possible to deserialize from a `&[u8]` without doing any copies. This
PR **stops short of actuallying doing that zero-copy deserialization**.
Instead, this PR is about adding the necessary trait impls to a variety
of types, along with a smattering of small refactorings to make rkyv
possible to use.

For those unfamiliar, rkyv works via the interplay of three traits:
`Archive`, `Serialize` and `Deserialize`. The usual flow of things is
this:

* Make a type `T` implement `Archive`, `Serialize` and `Deserialize`. rkyv
  helpfully provides `derive` macros to make this pretty painless in most
  cases.
* The process of implementing `Archive` for `T` *usually* creates an entirely
  new distinct type within the same namespace. One can refer to this type
  without naming it explicitly via `Archived<T>` (where `Archived` is a clever
  type alias defined by rkyv).
* Serialization happens from `T` to (conceptually) a `Vec<u8>`. The
  serialization format is specifically designed to reflect the in-memory layout
  of `Archived<T>`. Notably, *not* `T`. But `Archived<T>`.
* One can then get an `Archived<T>` with no copying (albeit, we will likely
  need to incur some cost for validation) from the previously created `&[u8]`.
  This is quite literally [implemented as a pointer cast][rkyv-ptr-cast].
* The problem with an `Archived<T>` is that it isn't your `T`. It's something
  else. And while there is limited interoperability between a `T` and an
  `Archived<T>`, the main issue is that the surrounding code generally demands
  a `T` and not an `Archived<T>`. **This is at the heart of the tension for
  introducing zero-copy deserialization, and this is mostly an intrinsic
  problem to the technique and not an rkyv-specific issue.** For this reason,
  given an `Archived<T>`, one can get a `T` back via an explicit
  deserialization step. This step is like any other kind of deserialization,
  although generally faster since no real "parsing" is required. But it will
  allocate and create all necessary objects.

This PR largely proceeds by deriving the three aforementioned traits
for `SimpleMetadata`. And, of course, all of its type dependencies. But
we stop there for now.

The main issue with carrying this work forward so that rkyv is actually
used to deserialize a `SimpleMetadata` is figuring out how to deal
with `DataWithCachePolicy` inside of the cached client. Ideally, this
type would itself have rkyv support, but adding it is difficult. The
main difficulty lay in the fact that its `CachePolicy` type is opaque,
not easily constructable and is internally the tip of the iceberg of
a rat's nest of types found in more crates such as `http`. While one
"dumb"-but-annoying approach would be to fork both of those crates
and add rkyv trait impls to all necessary types, it is my belief that
this is the wrong approach. What we'd *like* to do is not just use
rkyv to deserialize a `DataWithCachePolicy`, but we'd actually like to
get an `Archived<DataWithCachePolicy>` and make actual decisions used
the archived type directly. Doing that will require some work to make
`Archived<DataWithCachePolicy>` directly useful.

My suspicion is that, after doing the above, we may want to mush
forward with a similar approach for `SimpleMetadata`. That is, we want
`Archived<SimpleMetadata>` to be as useful as possible. But right
now, the structure of the code demands an eager conversion (and thus
deserialization) into a `SimpleMetadata` and then into a `VersionMap`.
Getting rid of that eagerness is, I think, the next step after dealing
with `DataWithCachePolicy` to unlock bigger wins here.

There are many commits in this PR, but most are tiny. I still encourage
review to happen commit-by-commit.

[rkyv]: https://rkyv.org/
[rkyv-ptr-cast]: https://docs.rs/rkyv/latest/src/rkyv/util/mod.rs.html#63-68


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-26 19:57_

---

_Review requested from @konstin by @BurntSushi on 2024-01-26 19:57_

---

_@charliermarsh reviewed on 2024-01-26 20:01_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:35 on 2024-01-26 20:01_

The benefits of having our own internal types separate from PyPI.

---

_Comment by @BurntSushi on 2024-01-26 20:06_

#1136 shows the diff between this PR and some hacks I've done to make it possible to get some kind of measurement for the impact of zero-copy deserialization on just `SimpleMetadata`. Effectively, I had to "stub" out `DataWithCachePolicy` and hard-code some caching decisions. But, this does let me get an idea of the impact here:

```
$ hyperfine -w5 --cleanup 'rm -rf /home/andrew/.cache/puffin' \
    "puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" \
    "PUFFIN_STUB_CACHE_POLICY=1 puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A kang
Benchmark 1: puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     280.0 ms ±   3.4 ms    [User: 258.8 ms, System: 127.2 ms]
  Range (min … max):   274.4 ms … 286.4 ms    10 runs

Benchmark 2: PUFFIN_STUB_CACHE_POLICY=1 puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     210.4 ms ±   2.1 ms    [User: 185.8 ms, System: 143.0 ms]
  Range (min … max):   207.5 ms … 213.1 ms    13 runs

Summary
  PUFFIN_STUB_CACHE_POLICY=1 puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.33 ± 0.02 times faster than puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

The speed-up isn't huge, but it's worth pointing out the following things:

* It removes `rmp_serde` completely, and I believe this is the primary thing getting us our speed-up here.
* While we can get an `Archived<SimpleMetadata>` (isomorphic to a `SimpleMetadataRaw` in that draft PR) without cost, we still need to do a deserialize step (for now) into a `SimpleMetadata` before we create a `VersionMap`. This is also where some cost is coming from.
* While I haven't investigated much, there's also now a fair bit of time being spent in dropping a `OnceMap`. (Haven't looked at this at all yet.)

---

_Label `internal` added by @BurntSushi on 2024-01-27 13:29_

---

_Label `performance` added by @BurntSushi on 2024-01-27 13:29_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:98 on 2024-01-28 15:18_

We could consider making this lazy. Internally, this method parses `url`, checks if it's absolute and, if not, joins `base` to it. So perhaps we should move the base parsing into `BaseUrl`?

---

_Review comment by @charliermarsh on `crates/distribution-filename/Cargo.toml`:23 on 2024-01-28 15:20_

How did you decide where / when to make this optional?

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:1131 on 2024-01-28 15:28_

(Seems like a good change regardless.)

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/finder.rs`:179 on 2024-01-28 15:29_

I personally would prefer `version_wheel` and `version_sdist` or `version_source_dist`, but defer to you as author :)

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:205 on 2024-01-28 15:30_

Can you explain this, for my understanding? (I probably didn't understand this even when I wrote it, and was just following Tokio examples.)

---

_@charliermarsh approved on 2024-01-28 15:33_

Very nice! I actually thought this would end up being even _more_ invasive than it played out in this PR.

Is your plan to merge this as-is, or wait for the following PRs to play out?

---

_@BurntSushi reviewed on 2024-01-28 16:08_

---

_Review comment by @BurntSushi on `crates/distribution-filename/Cargo.toml`:23 on 2024-01-28 16:08_

I copied whether `serde` was optional or not. (And I don't think I understand the reasoning for when `serde` is optional versus when it isn't. It seems like maybe it is made optional for crates that aren't tightly coupled to puffin.)

IMO, we should make `serde` (and `rkyv`) required everywhere. If and when we ever publish any of our crates, then we can make things optional (and possibly do other things that would be appropriate for an "ecosystem" crate).

---

_@BurntSushi reviewed on 2024-01-28 16:12_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/finder.rs`:179 on 2024-01-28 16:12_

I don't feel too strongly. I can change them. I usually like to keep the length of variable names roughly proportional to their scope. But this is a big loop.

---

_@BurntSushi reviewed on 2024-01-28 16:25_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:205 on 2024-01-28 16:25_

I actually don't have easy access to a root cause explanation here. The general idea is that explicit pin is only needed when the thing you're using is `!Unpin`. [Most things in the Rust universe implement `Unpin`](https://doc.rust-lang.org/std/marker/trait.Unpin.html). The key thing, AIUI, that is not `Unpin` are async tasks with borrows in them. But these don't typically manifest themselves as a named type. (Like closures.) So when you get an async task that is `!Unpin` and need to do something with it, it's usually _then_ that you need to pin it.

So answering your question in this context is somewhat like answering a negative. We'd have to go down the stack through all async tasks and basically note, "yeah this doesn't have any borrows in it, so it is `Unpin` and thus doesn't need to be pinned."

And then of course, this whole pinning thing is needed because async tasks might get moved around in memory. But they might also have "self" references to things on their stack. So in order for moving those tasks to be safe, you need to guarantee that all of those self references will continue to be valid after the task gets moved. And the way to do that is to "pin" it, and usually that introducing indirection (i.e., a pointer) of some kind.

(I am overall about 95% confident in this explanation.)

---

_@BurntSushi reviewed on 2024-01-28 16:55_

---

_Review comment by @BurntSushi on `crates/puffin-distribution/src/distribution_database.rs`:98 on 2024-01-28 16:55_

So I don't think it's worth doing this for perf reasons. It doesn't show up on a profile as far as I can see. So I'm not sure it makes sense to spend the extra effort to optimize it.

But I do think it makes the code clearer here. So I did it for those reasons. :) See: https://github.com/astral-sh/puffin/pull/1135/commits/8a5a0aff14173ba4584af70b392814e789a2e124

---

_Comment by @BurntSushi on 2024-01-28 16:56_

> Is your plan to merge this as-is, or wait for the following PRs to play out?

No I definitely want to merge it unless there are objections. I want to merge #1150 too. I devised both PRs to intentionally stop short of actually changing how puffin works. But it's helpful to get this work on to `main`.

---

_Merged by @BurntSushi on 2024-01-28 17:14_

---

_Closed by @BurntSushi on 2024-01-28 17:14_

---

_Branch deleted on 2024-01-28 17:15_

---

_Comment by @BurntSushi on 2024-01-28 18:12_

> I actually thought this would end up being even _more_ invasive than it played out in this PR.

It's partially due to `rkyv` providing a very nice set of `derive` macros, and partially because this is only supporting (not actually doing, yet) zero-copy deseriliazation "at the edges." The invasive part comes when we, for example, want to use `Archived<SimpleMetadata>` (or similar) in more places than "use it to eagerly build a `VersionMap`." Essentially, the invasive part comes from the tension between `T` and `Archived<T>`. You want the latter---because it takes no copies or explicit deserialization to produce---but the surrounding code wants `T`.

---
